# Requirements of the Typeahead Suggestion System’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

In this lesson, we look into the requirements and estimated resources that are necessary for the design of the typeahead suggestion system. Our proposed design should meet the following requirements.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

The system should suggest top N (let’s say top ten) frequent and relevant terms to the user based on the text a user types in the search box.

#### Non-functional requirements <a href="#non-functional-requirements-2" id="non-functional-requirements-2"></a>

* **Low latency:** The system should show all the suggested queries in real time after a user types. The latency shouldn’t exceed 200 ms. A study suggests that the average time between two keystrokes is 160 milliseconds. So, our time-budget of suggestions should be greater than 160 ms to give a real-time response. This is because if a user is typing fast, they already know what to search and might not need suggestions. At the same time, our system response should be greater than 160 ms. However, it should not be too high because in that case, a suggestion might be stale and less useful.
* **Fault tolerance:** The system should be reliable enough to provide suggestions despite the failure of one or more of its components.
* **Scalability:** The system should support the ever-increasing number of users over time.

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

As was stated earlier, the typeahead feature is used to enhance the user experience while typing a query. We need to design a system that works on a scale that’s similar to Google Search. Google receives more than 3.5 billion searches every day. Designing such an enormous system is a challenging task that requires different resources. Let’s estimate the storage and bandwidth requirements for the proposed system.

#### Storage estimation <a href="#storage-estimation-1" id="storage-estimation-1"></a>

Assuming that out of the 3.5 billion queries per day, two billion queries are unique and need to be stored. Let’s also assume that each query consists of 15 characters on average, and each character takes 2 Bytes of storage. According to this formulation, we would require the following:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.10.05 AM.png" alt=""><figcaption></figcaption></figure>

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

3.5 billion queries will reach our system every day. Assume that each query a user types is 15 characters long on average.

Keeping this in mind, the total number of reading requests of characters per day would be as follows:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.10.46 AM.png" alt=""><figcaption></figcaption></figure>

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

Our system will receive 607,000 requests per second concurrently. Therefore, we need to have many servers installed to avoid burdening a single server. Let’s assume that a single server can handle 8,000 queries per second. So, we require around 76 servers to handle 607,000 queries.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.11.14 AM.png" alt=""><figcaption></figcaption></figure>

In the table below, adjust the values to see how the resource estimations change.

**Resource Estimation for the Typeahead Suggestion System**

| Total Queries per Day         | 3.5    | billion            |
| ----------------------------- | ------ | ------------------ |
| Unique Queries per Day        | 2      | billion            |
| Minimum Characters in a Query | 15     | Characters         |
| Server's QPS                  | 8000   | Queries per second |
| Storage                       | f60    | GB/day             |
| Incoming Bandwidth            | f9.7   | Mb/sec             |
| Outgoing Bandwidth            | f1.455 | Gb/sec             |
| Number of Servers             | f76    | Severs             |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of the typeahead suggestion system consists of the following building blocks that have been discussed in the initial chapters of the course:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 2.12.51 AM.png" alt=""><figcaption></figcaption></figure>

* [**Databases**](../databases/introduction-to-databases.md) are required to keep the data related to the queries’ prefixes.
* [**Load balancers**](../load-balancers/introduction-to-load-balancers.md) are required to disseminate incoming queries among a number of active servers.
* [**Caches**](../distributed-cache/system-design-the-distributed-cache.md) are used to keep the top �N suggestions for fast retrieval.

In the next lesson, we’ll focus on the high-level design and APIs of the typeahead suggestion system.
