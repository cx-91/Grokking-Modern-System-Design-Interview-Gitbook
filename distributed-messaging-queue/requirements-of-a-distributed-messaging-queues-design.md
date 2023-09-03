# Requirements of a Distributed Messaging Queue’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

In a **distributed messaging queue**, data resides on several machines. Our aim is to design a distributed messaging queue that has the following functional and non-functional requirements.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

Listed below are the actions that a client should be able to perform:

* **Queue creation:** The client should be able to create a queue and set some parameters—for example, queue name, queue size, and maximum message size.
* **Send message:** Producer entities should be able to send messages to a queue that’s intended for them.
* **Receive message:** Consumer entities should be able to receive messages from their respective queues.
* **Delete message:** The consumer processes should be able to delete a message from the queue after a successful processing of the message.
* **Queue deletion:** Clients should be able to delete a specific queue.

#### Non-functional requirements <a href="#non-functional-requirements-0" id="non-functional-requirements-0"></a>

Our design of a distributed messaging queue should adhere to the following non-functional requirements:

* **Durability**: The data received by the system should be durable and shouldn’t be lost. Producers and consumers can fail independently, and a queue with data durability is critical to make the whole system work, because other entities are relying on the queue.
* **Scalability**: The system needs to be scalable and capable of handling the increased load, queues, producers, consumers, and the number of messages. Similarly, when the load reduces, the system should be able to shrink the resources accordingly.
* **Availability**: The system should be highly available for receiving and sending messages. It should continue operating uninterrupted, even after the failure of one or more of its components.
* **Performance:** The system should provide high throughput and low latency.

### Single-server messaging queue <a href="#single-server-messaging-queue-0" id="single-server-messaging-queue-0"></a>

Before we embark on our journey to map out the design of a distributed messaging queue, we should recall how queues are used within a single server where the producer and consumer processes are also on the same node. A producer or consumer can access a single-server queue by acquiring the locking mechanism to avoid data inconsistency. The queue is considered a critical section where only one entity, either the producer or consumer, can access the data at a time.

However, several aspects restrain us from using the single-server messaging queue in today’s distributed systems paradigm. For example, it becomes unavailable to cooperating processes, producers and consumers, in the event of hardware or network failures. Moreover, performance takes a major hit as contention on the lock increases. Furthermore, it is neither scalable nor durable.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 12.44.59 AM.png" alt=""><figcaption></figcaption></figure>

**Question**

Can we extend the design of a single-server messaging queue to a distributed messaging queue?

A single-server messaging queue has the following drawbacks:

1. **High latency:** As in the case of a single-server messaging queue, a producer or consumer acquires a lock to access the queue. Therefore, this mechanism becomes a bottleneck when many processes try to access the queue. This increases the latency of the service.
2. **Low availability:** Due to the lack of replication of the messaging queue, the producer and consumer process might be unable to access the queue in events of failure. This reduces the system’s availability and reliability.
3. **Lack of durability:** Due to the absence of replication, the data in the queue might be lost in the event of a system failure.
4. **Scalability:** A single-server messaging queue can handle a limited number of messages, producers, and consumers. Therefore, it is not scalable.

To extend the design of a single-server messaging queue to a distributed messaging queue, we need to make extensive efforts to eliminate the drawbacks outlined above.

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of a distributed messaging queue utilizes the following building blocks:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 12.45.17 AM.png" alt=""><figcaption></figcaption></figure>

* [**Database(s)**](https://www.educative.io/collection/page/10370001/4941429335392256/4901035478351872) will be required to store the metadata of queues and users.
* [**Caches**](https://www.educative.io/collection/page/10370001/4941429335392256/5053577315221504) are important to keep frequently accessed data, whether it be data pertaining to users or queues metadata.
* [**Load balancers**](https://www.educative.io/collection/page/10370001/4941429335392256/4521972679049216) are used to direct incoming requests to servers where the metadata is stored.

In our discussion on messaging queues, we focused on their functional and non-functional requirements. Before moving on to the process of designing a distributed messaging queue, it’s essential for us to discuss some key considerations and challenges that may affect the design.
