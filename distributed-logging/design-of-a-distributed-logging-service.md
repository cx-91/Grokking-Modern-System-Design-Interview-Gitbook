# Design of a Distributed Logging Service

We’ll design the distributed logging system now. Our logging system should log all activities or messages (we’ll not incorporate sampling ability into our design).

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Let’s list the requirements for designing a distributed logging system:

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

The functional requirements of our system are as follows:

* **Writing logs**: The services of the distributed system must be able to write into the logging system.
* **Searchable logs**: It should be effortless for a system to find logs. Similarly, the application’s flow from end-to-end should also be effortless.
* **Storing logging**: The logs should reside in distributed storage for easy access.
* **Centralized logging visualizer**: The system should provide a unified view of globally separated services.

#### Non-functional requirements <a href="#non-functional-requirements-2" id="non-functional-requirements-2"></a>

The non-functional requirements of our system are as follows:

* **Low latency**: Logging is an I/O-intensive operation that is often much slower than CPU operations. We need to design the system so that logging is not on an application’s critical path.
* **Scalability:** We want our logging system to be scalable. It should be able to handle the increasing amounts of logs over time and a growing number of concurrent users.
* **Availability:** The logging system should be highly available to log the data.

### Building blocks we will use <a href="#building-blocks-we-will-use-3" id="building-blocks-we-will-use-3"></a>

The design of a distributed logging system will utilize the following building blocks:

* [**Pub-sub system**](https://www.educative.io/pageeditor/10370001/4941429335392256/4996814243889152): We’ll use a pub-sub- system to handle the huge size of logs.
* [**Distributed search**](https://www.educative.io/pageeditor/10370001/4941429335392256/5400897294696448): We’ll use distributed search to query the logs efficiently.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.37.44 AM.png" alt=""><figcaption></figcaption></figure>

### API design <a href="#api-design-0" id="api-design-0"></a>

The API design for this problem is given below:

**Write a message**

The API call to perform writing should look like this:

```txt
write(unique_ID, message_to_be_logged)
```

| **Parameter**          | **Description**                                                                 |
| ---------------------- | ------------------------------------------------------------------------------- |
| `unique_ID`            | It is a numeric ID containing `application-id`, `service-id,` and a time stamp. |
| `message_to_be_logged` | It is the log message stored against a unique key.                              |

**Search log**

The API call to search data should look like this:

```txt
searching(keyword)
```

This call returns a list of logs that contain the keyword.

| **Parameter** | **Description**                                           |
| ------------- | --------------------------------------------------------- |
| `keyword`     | It is used for finding the logs containing the `keyword`. |

### Initial design <a href="#initial-design-0" id="initial-design-0"></a>

In a distributed system, clients across the globe generate events by requesting services from different serving nodes. The nodes generate logs while handling each of the requests. These logs are accumulated on the respective nodes.

In addition to the building blocks, let’s list the major components of our system:

* **Log accumulator**: An agent that collects logs from each node and dumps them into storage. So, if we want to know about a particular event, we don’t need to visit each node, and we can fetch them from our storage.
* **Storage**: The logs need to be stored somewhere after accumulation. We’ll choose blob storage to save our logs.
* **Log indexer**: The growing number of log files affects the searching ability. The log indexer will use the distributed search to search efficiently.
* **Visualizer**: The visualizer is used to provide a unified view of all the logs.

The design for this method looks like this:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.38.44 AM.png" alt=""><figcaption></figcaption></figure>

There are millions of servers in a distributed system, and using a single log accumulator severely affects scalability. Let’s learn how we’ll scale our system.

### Logging at various levels <a href="#logging-at-various-levels-0" id="logging-at-various-levels-0"></a>

Let’s explore how the logging system works at various levels.

#### In a server <a href="#in-a-server-1" id="in-a-server-1"></a>

In this section, we’ll learn how various services belonging to different apps will log in to a server.

Let’s consider a situation where we have multiple different applications on a server, such as App 1, App 2, and so on. Each application has various microservices running as well. For example, an e-commerce application can have services like authenticating users, fetching carts, and more running at the same time. Every service produces logs. We use an ID with `application-id`, `service-id`, and its time stamp to uniquely identify various services of multiple applications. Time stamps can help us to determine the causality of events.

Each service will push its data to the **log accumulator** service. It is responsible for these actions:

* Receiving the logs.
* Storing the logs locally.
* Pushing the logs to a [pub-sub system](../pub-sub/system-design-the-pub-sub-abstraction.md).

We use the pub-sub system to cater to our scalability issue. Now, each server has its log accumulator (or multiple accumulators) push the data to pub-sub. The pub-sub system is capable of managing a huge amount of logs.

To fulfill another requirement of low latency, we don’t want the logging to affect the performance of other processes, so we send the logs asynchronously via a low-priority thread. By doing this, our system does not interfere with the performance of others and ensures availability.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.38.44 AM (1).png" alt=""><figcaption></figcaption></figure>

We should be mindful that data can be lost in the process of logging huge amounts of messages. There is a trade-off between user-perceived latency and the guarantee that log data persists. For lower latency, log services often keep data in RAM and persist them asynchronously. Additionally, we can minimize data loss by adding redundant log accumulators to handle growing concurrent users.

**Question**

How does logging change when we host our service on a multi-tenant cloud (like AWS) versus when an organization has exclusive control of the infrastructure (like Facebook), specifically in terms of logs?

Security might be one aspect that differs between multi-tenant and single-tenant settings. When we encrypt all logs and secure a logging service end-to-end, it does not come free, and has performance penalties. Additionally, strict separation of logs is required for a multi-tenant setting, while we can improve the storage and processing utilization for a single-tenant setting.

Let’s take the example of Meta’s Facebook. They have millions of machines that generate logs, and the size of the logs can be several petabytes per hour. So, each machine pushes its logs to a pub-sub system named Scribe. Scribe retains data for a few days and various other systems process the information residing in the **Scribe**. They store the logs in distributed storage also. Managing the logs can be application-specific.

On the other hand, for multi-tenancy, we need a separate instance of pub-sub per tenant (or per application) for strict separation of logs.

\-----------

> **Note:** For applications like banking and financial apps, the logs must be very secure so hackers cannot steal the data. The common practice is to encrypt the data and log. In this way, no one can decrypt the encrypted information using the data from logs.

#### At datacenter level <a href="#at-datacenter-level-0" id="at-datacenter-level-0"></a>

All servers in a data center push the logs to a pub-sub system. Since we use a horizontally-scalable pub-sub system, it is possible to manage huge amounts of logs. We may use multiple instances of the pub-sub per data center. It makes our system scalable, and we can avoid bottlenecks. Then, the pub-sub system pushes the data to the blob storage.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.40.58 AM.png" alt=""><figcaption></figcaption></figure>

The data does not reside in pub-sub forever and gets deleted after a few days before being stored in archival storage. However, we can utilize the data while it is available in the pub-sub system. The following services will work on the pub-sub data:

* **Filterer**: It identifies the application and stores the logs in the blob storage reserved for that application since we do not want to mix logs of two different applications.
* **Error aggregator**: It is critical to identify an error as quickly as possible. We use a service that picks up the error messages from the pub-sub system and informs the respective client. It saves us the trouble of searching the logs.
* **Alert aggregator**: Alerts are also crucial. So, it is important to be aware of them early. This service identifies the alerts and notifies the appropriate stakeholders if a fatal error is encountered, or sends a message to a monitoring tool.

The updated design is given below:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.41.24 AM.png" alt=""><figcaption></figcaption></figure>

**Question**

Do we store the logs for a lifetime?

Logs also have an expiration date. We can delete regular logs after a few days or months. Compliance logs are usually stored for up to three to five years. It depends on the requirements of the application.

\----------------

In our design, we have identified another component called the **expiration checker**. It is responsible for these tasks:

* Verifying the logs that have to be deleted. Verifying the logs to store in cold storage.

Moreover, our components log indexer and visualizer work on the blob storage to provide a good searching experience to the end user. We can see the final design of the logging service below:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.42.03 AM.png" alt=""><figcaption></figcaption></figure>

**Question**

We learned earlier that a simple user-level API call to a large service might involve hundreds of internal microservices and thousands of nodes. How can we stitch together logs end-to-end for one request with causality intact?

Most complex services use a front-end server to handle an end user’s request. On reception of a request, the front-end server can get a unique identifier using a sequencer. This unique identifier will be appended to all the fanned-out services. Each log message generated anywhere in the system also emits the unique identifier.

Later, we can filter the log (or preprocess it) based on the unique identifiers. At this step, we are able to collect all the logs across microservices against a unique request. In the [Sequencer](https://www.educative.io/collection/page/10370001/4941429335392256/6499939719053312) building block, we discussed that we can get unique identifiers that maintain happens-before causality. Such an identifier has the property that if ID 1 is less than ID 2, then ID 1 represents a time that occurred before ID 2. Now, each log item can use a time- stamp, and we can sort log entries for a specific request in ascending order.

Correctly ordering the log in a chronological (or causal) order simplifies log analyses.

\-------------

> **Note:** Windows Azure Storage System (WAS) uses an extensive logging infrastructure in its development. It stores the logs in local disks, and given a large number of logs, they do not push the logs to the distributed storage. Instead, they use a grep-like utility that works as a distributed search. This way, they have a unified view of globally distributed logs data.

There can be various ways to design a distributed logging service, but it solely depends on the requirements of our application.

### Conclusion <a href="#conclusion-0" id="conclusion-0"></a>

* We learned how logging is crucial in understanding the flow of events in a distributed system. It helps to reduce the mean time to repair (MTTR) by steering us toward the root causes of issues.
* Logging is an I/O-intensive operation that is time-consuming and slow. It is essential to handle it carefully and not affect the critical path of other services’ execution.
* Logging is essential for monitoring because the data fetched from logs helps monitor the health of an application. (Alert and error aggregators serve this purpose.)
