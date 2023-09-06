# Detailed Design of WhatsApp

### Detailed design <a href="#detailed-design-0" id="detailed-design-0"></a>

The high-level design discussed in the previous lesson doesn’t answer the following questions:

* How is a communication channel created between clients and servers?
* How can the high-level design be scaled to support billions of users?
* How is the user’s data stored?
* How is the receiver identified to whom the message is delivered?

To answer all these questions, let’s dive deep into the high-level design and explore each component in detail. Let’s start with how users make connections with the chat servers.

#### Connection with a WebSocket server <a href="#connection-with-a-websocket-server-0" id="connection-with-a-websocket-server-0"></a>

In WhatsApp, each active device is connected with a **WebSocket server** via WebSocket protocol. A WebSocket server keeps the connection open with all the active (online) users. Since one server isn’t enough to handle billions of devices, there should be enough servers to handle billions of users. The responsibility of each of these servers is to provide a port to every online user. The mapping between servers, ports, and users is stored in the WebSocket manager that resides on top of a cluster of the data store. In this case, that’s Redis.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.57.12 AM.png" alt=""><figcaption></figcaption></figure>

**Question**

Why is WebSocket preferred over HTTP(S) protocol for client-server communication?

HTTP(S) doesn’t keep the connection open for the servers to send frequent data to a client. With HTTP(S) protocol, a client constantly requests updates from the server, commonly called **polling**, which is resource intensive and causes latency. WebSocket maintains a persistent connection between the client and a server. This protocol transfers data to the client immediately whenever it becomes available. It provides a bidirectional connection used as a common solution to send asynchronous updates from a server to a client.

\-------------------

#### Send or receive messages <a href="#send-or-receive-messages-0" id="send-or-receive-messages-0"></a>

The WebSocket manager is responsible for maintaining a mapping between an active user and a port assigned to the user. Whenever a user is connected to another WebSocket server, this information will be updated in the data store.

A WebSocket server also communicates with another service called message service. **Message service** is a repository of messages on top of the Mnesia database cluster. It acts as an interface to the Mnesia database for other services interacting with the databases. It is responsible for storing and retrieving messages from the Mnesia database. It also deletes messages from the Mnesia database after a configurable amount of time. And, it exposes APIs to receive messages by various filters, such as user ID, message ID, and so on.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.58.52 AM.png" alt=""><figcaption></figcaption></figure>

Now, let’s assume that user A wants to send a message to user B. As shown in the above figure, both users are connected to different WebSocket servers. The system performs the following steps to send messages from user A to user B:

1. User A communicates with the corresponding WebSocket server to which it is connected.
2. The WebSocket server associated with user A identifies the WebSocket to which user B is connected via the WebSocket manager. If user B is online, the WebSocket manager responds back to user A’s WebSocket server that user B is connected with its WebSocket server.
3. Simultaneously, the WebSocket server sends the message to the message service and is stored in the Mnesia database where it gets processed in the first-in-first-out order. As soon as these messages are delivered to the receiver, they are deleted from the database.
4. Now, user A’s WebSocket server has the information that user B is connected with its own WebSocket server. The communication between user A and user B gets started via their WebSocket servers.
5. If user B is offline, messages are kept in the Mnesia database. Whenever they become online, all the messages intended for user B are delivered via push notification. Otherwise, these messages are deleted permanently after 30 days.

Both users (sender and receiver) communicate with the WebSocket manager to find each other’s WebSocket server. Consider a case where there can be a continuous conversation between both users. This way, many calls are made to the WebSocket manager. To minimize the latency and reduce the number of these calls to the WebSocket manager, each WebSocket server caches the following information:

* If both users are connected to the same server, the call to the WebSocket manager is avoided.
* It caches information of recent conversations about which user is connected to which WebSocket server.

**Question**

The data in the cache will become outdated if a user gets disconnected and connects with another server. Keeping this in mind, how long should a WebSocket server cache information?

The information will be updated in the WebSocket manager when a user disconnects due to some faults in the connection and reconnects with a different WebSocket server. The WebSocket manager, in turn, invalidates the data in the cache used by the WebSocket servers, and the updated data is sent to the corresponding cache. So, the information in the cache will remain there until it receives an invalidate signal from the WebSocket manager.

\-----------------

#### Send or receive media files <a href="#send-or-receive-media-files-0" id="send-or-receive-media-files-0"></a>

So far, we’ve discussed the communication of text messages. But what happens when a user sends media files? Usually, the WebSocket servers are lightweight and don’t support heavy logic such as handling the sending and receiving of media files. We have another service called the **asset service**, which is responsible for sending and receiving media files.

Moreover, the sending of media files consists of the following steps:

1. The media file is compressed and encrypted on the device side.
2. The compressed and encrypted file is sent to the asset service to store the file on blob storage. The asset service assigns an ID that’s communicated with the sender. The asset service also maintains a hash for each file to avoid duplication of content on the blob storage. For example, if a user wants to upload an image that’s already there in the blob storage, the image won’t be uploaded. Instead, the same ID is forwarded to the receiver.
3. The asset service sends the ID of media files to the receiver via the message service. The receiver downloads the media file from the blob storage using the ID.
4. The content is loaded onto a CDN if the asset service receives a large number of requests for some particular content.

The following figure demonstrates the components involved in sharing media files over WhatsApp messenger:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.00.56 AM.png" alt=""><figcaption></figcaption></figure>

#### Support for group messages <a href="#support-for-group-messages" id="support-for-group-messages"></a>

WebSocket servers don’t keep track of groups because they only track active users. However, some users could be online and others could be offline in a group. For group messages, the following three main components are responsible for delivering messages to each user in a group:

* Group message handler
* Group message service
* Kafka

Let’s assume that user A wants to send a message to a group with some unique ID—for example, `Group/A`. The following steps explain the flow of a message sent to a group:

1. Since user A is connected to a WebSocket server, it sends a message to the message service intended for `Group/A`.
2. The message service sends the message to Kafka with other specific information about the group. The message is saved there for further processing. In Kafka terminology, a group can be a topic, and the senders and receivers can be producers and consumers, respectively.
3. Now, here comes the responsibility of the group service. The **group service** keeps all information about users in each group in the system. It has all the information about each group, including user IDs, group ID, status, group icon, number of users, and so on. This service resides on top of the MySQL database cluster, with multiple secondary replicas distributed geographically. A Redis cache server also exists to cache data from the MySQL servers. Both geographically distributed replicas and Redis cache aid in reducing latency.
4. The group message handler communicates with the group service to retrieve data of `Group/A` users.
5. In the last step, the group message handler follows the same process as a WebSocket server and delivers the message to each user.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.01.57 AM.png" alt=""><figcaption></figcaption></figure>

\----------------------------

Optional: How encryption and decryption work in WhatsApp?

#### End-to-end encryption

Similar to the group message service, there’s another service known as **user service** that keeps track of each user. It contains user-related data, such as name, profile picture, status, and so on. This service resides on top of the MySQL database and utilizes cache for frequently used data.

At registration time, a WhatsApp client generates a number of public-private key pairs that include keys pairs for identity, a signed prekey, and a batch of one-time prekeys. These keys are used for different purposes. The public keys are stored in the database associated with the user service along with the user’s identifier. Whenever a sender wants to establish a connection with the receiver, it also requests the server for the keys associated with a receiver’s ID. Private keys are only stored on the client’s end devices.

**One-to-one communication**

To communicate securely, WhatsApp clients, on both sender and receiver sides, establish a pairwise encrypted session with each other. The session is created using the following steps:

1. The sender requests the public identity key, signed prekey, and a single public one-time prekey of the receiver’s device. The one-time prekey is used only once.
2. The WhatsApp server returns the requested public keys of the receiver.
3. The sender generates an ephemeral Curve25519 key pair.
4. The sender creates a master secret key using its own identity key, the ephemeral key pair, and the receiver’s public keys.
5. The sender uses the HMAC-based key-derivation function (HKDF) to create two other keys known as a root key and chain key from the master secret key. The sender starts sending messages to the receiver after establishing the pairwise encrypted session. The receiver creates the same session based on the information present in the header of the first receiving message.
6. After establishing a session, an 80-byte `Message Key` is generated based on the root and chain Keys. The `Message Key` is used for encryption and authentication of messages.

**Group communication**

The communication in a WhatsApp group is handled by the same process discussed in the previous section on one-to-one communication. A sender key, used by the signaling messaging protocol, is generated and shared with each member’s device of the group using the pairwise encrypted session. The messages sent to a group are encrypted using the sender key.

The communication in a group is directed by Kafka, where each user in a group subscribes to the relevant topic (a group with an associated messaging queue). The messages are then delivered in a fanout messaging pattern where messages are delivered from a single sender to multiple receivers.

#### Simultaneous maintenance of WhatsApp sessions on multiple devices

Every device on WhatsApp is identified by a device identity key. If a user uses multiple devices (a primary and several other companion devices), each device identity key is stored against the user’s account on the WhatsApp server. When the WhatsApp server receives a message from a user, it transmits messages multiple times to each device linked with a user’s account.

Similarly, each device used by a user has its own set of encryption keys. If a set of keys of one companion device is compromised, the attacker won’t be able to see messages communicated with the other device.

We’ve just touched the basics here because security is a deep topic. See the WhatsApp security whitepaper for further details.

\-----------------

### Put everything together <a href="#put-everything-together-0" id="put-everything-together-0"></a>

We discussed the features of our WhatsApp system design. It includes user connection with a server, sending messages and media files, group messages, and end-to-end encryption, individually. The final design of our WhatsApp messenger is as follows:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.03.35 AM.png" alt=""><figcaption></figcaption></figure>

In the next lesson, we’ll evaluate our design and look into the non-functional requirements.
