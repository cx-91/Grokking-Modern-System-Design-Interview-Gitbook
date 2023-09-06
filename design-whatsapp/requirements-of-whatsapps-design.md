# Requirements of WhatsApp’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Our design of the WhatsApp messenger should meet the following requirements.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

* **Conversation**: The system should support one-on-one and group conversations between users.
* **Acknowledgment**: The system should support message delivery acknowledgment, such as sent, delivered, and read.
* **Sharing**: The system should support sharing of media files, such as images, videos, and audio.
* **Chat storage**: The system must support the persistent storage of chat messages when a user is offline until the successful delivery of messages.
* **Push notifications**: The system should be able to notify offline users of new messages once their status becomes online.

#### Non-functional requirements <a href="#non-functional-requirements-0" id="non-functional-requirements-0"></a>

* **Low latency**: Users should be able to receive messages with low latency.
* **Consistency**: Messages should be delivered in the order they were sent. Moreover, users must see the same chat history on all of their devices.
* **Availability**: The system should be highly available. However, the availability can be compromised in the interest of consistency.
* **Security**: The system must be secure via end-to-end encryption. The end-to-end encryption ensures that only the two communicating parties can see the content of messages. Nobody in between, not even WhatsApp, should have access.
* **Scalability:** The system should be highly scalable to support an ever-increasing number of users and messages per day.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.41.30 AM.png" alt=""><figcaption></figcaption></figure>

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

WhatsApp is the most used messaging application across the globe. According to WhatsApp, it supports more than two billion users around the world who share more than 100 billion messages each day. We need to estimate the storage capacity, bandwidth, and number of servers to support such an enormous number of users and messages.

#### Storage estimation <a href="#storage-estimation-0" id="storage-estimation-0"></a>

As there are more than 100 billion messages shared per day over WhatsApp, let’s estimate the storage capacity based on this figure. Assume that each message takes 100 Bytes on average. Moreover, the WhatsApp servers keep the messages only for 30 days. So, if the user doesn’t get connected to the server within these days, the messages will be permanently deleted from the server.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.42.01 AM.png" alt=""><figcaption></figcaption></figure>

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.43.00 AM.png" alt=""><figcaption></figcaption></figure>

### High-level Estimates

| **Type**                 | **Estimates** |
| ------------------------ | ------------- |
| Total messages per day   | 100 billion   |
| Storage required per day | 10 TB         |
| Storage for 30 days      | 300 TB        |
| Incoming data per second | 926 Mb/s      |
| Outgoing data per second | 926 Mb/s      |

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

WhatsApp handles around 10 million connections on a single server, which seems quite high for a server. However, it’s possible by extensive performance engineering. We’ll need to know all the in-depth details of a system, such as a server’s kernel, networking library, infrastructure configuration, and so on.

> **Note:** We can often optimize a general-purpose server for special tasks by careful performance engineering of the full software stack.

Let’s move to the estimation of the number of servers:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.44.33 AM.png" alt=""><figcaption></figcaption></figure>

#### Try it out <a href="#try-it-out-0" id="try-it-out-0"></a>

Let’s analyze how the number of messages per day affects the storage and bandwidth requirements. For this purpose, we can change values in the following table to compute the estimates:



| Number of users per day (in billions)                   | 2      |
| ------------------------------------------------------- | ------ |
| Number of messages per day (in billions)                | 100    |
| Size of each message (in bytes)                         | 100    |
| Number of connections a server can handle (in millions) | 10     |
| Storage estimation per day (in TB)                      | f10    |
| Incoming and Outgoing bandwidth (Mb/s)                  | f926.4 |
| Number of chat servers required                         | f200   |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of WhatsApp utilizes the following building blocks that have also been discussed in the initial chapters:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.45.25 AM.png" alt=""><figcaption></figcaption></figure>

* [**Databases**](../databases/introduction-to-databases.md) are required to store users’ and groups’ metadata.
* [**Blob storage**](../blob-store/system-design-a-blob-store.md) is used to store multimedia content shared in messages.
* A [**CDN**](../content-delivery-network-cdn/system-design-the-content-delivery-network-cdn.md) is used to effectively deliver multimedia content that’s frequently shared.
* A [**load balancer**](../load-balancers/introduction-to-load-balancers.md) distributes incoming requests among the pool of available servers.
* [**Caches**](../distributed-cache/system-design-the-distributed-cache.md) are required to keep frequently accessed data used by various services.
* A [**messaging queue**](../distributed-messaging-queue/system-design-the-distributed-messaging-queue.md) is used to temporarily keep messages in a queue on a database when a user is offline.

In the next lesson, we’ll focus on the high-level design of the WhatsApp messenger.
