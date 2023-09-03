# Requirements of a Web Crawler's Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Let’s highlight the functional and non-functional requirements of a web crawler.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

These are the functionalities a user must be able to perform:

* **Crawling**: The system should scour the WWW, spanning from a queue of seed URLs provided initially by the system administrator.

Points to Ponder

**Question 1**

Where do we get these seed URLs from?

Show Answer

**1 of 3**

* **Storing**: The system should be able to extract and store the content of a URL in a blob store. This makes that URL and its content processable by the search engines for indexing and ranking purposes.
* **Scheduling**: Since crawling is a process that’s repeated, the system should have regular scheduling to update its blob stores’ records.

#### Non-functional requirements <a href="#non-functional-requirements-0" id="non-functional-requirements-0"></a>

* **Scalability**: The system should inherently be distributed and multithreaded, because it has to fetch hundreds of millions of web documents.
* **Extensibility**: Currently, our design supports HTTP(S) communication protocol and text files storage facilities. For augmented functionality, it should also be extensible for different network communication protocols, able to add multiple modules to process, and store various file formats.
*   **Consistency**: Since our system involves multiple crawling workers, having data consistency among all of them is necessary.

    In the general context, data consistency means the reliability and accuracy of data across a system or dataset. In the web crawler’s context, it refers to the adherence of all the workers to a specific set of rules in their attempt to generate consistent crawled data.
* **Performance**: The system should be smart enough to limit its crawling to a domain, either by time spent or by the count of the visited URLs of that domain. This process is called **self-throttling**. The URLs crawled per second and the throughput of the content crawled should be optimal.

Tip

* **Improved user interface—customized scheduling**: Besides the default recrawling, which is a functional requirement, the system should also support the functionality to perform non-routine customized crawling on the system administrator’s demands.

The non-functional requirements of the web crawler system

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

We need to estimate various resource requirements for our design.

**Assumptions**

These are the assumptions we’ll use when estimating our resource requirements:

* There are a total of 5 billion web pages.
* The text content per webpage is 2070 KB.
* The metadata for one web page is 500 Bytes.

#### Storage estimation <a href="#storage-estimation-1" id="storage-estimation-1"></a>

The collective storage required to store the textual content of 5 billion web pages is: ����� ������� ��� �����=5 �������×(2070 ��+500�)=10.35��Total storage per crawl=5 Billion×(2070 KB+500B)=10.35PB

The total storage required by the web crawler system

#### Traversal time <a href="#traversal-time-0" id="traversal-time-0"></a>

Since the traversal time is just as important as the storage requirements, let’s calculate the approximate time for one-time crawling. Assuming that the average HTTP traversal per webpage is 60 ms, the time to traverse all 5 billion pages will be:

����� ��������� ����=5 �������×60 ��=0.3 ������� �������=Total traversal time=5 Billion×60 ms=0.3 Billion seconds= 9.5 �����9.5 years

It’ll take approximately 9.5 years to traverse the whole Internet while using one instance of crawling, but we want to achieve our goal in one day. We can accomplish this by designing our system to support multi-worker architecture and divide the tasks among multiple workers running on different servers.

#### Number of servers estimation for multi-worker architecture <a href="#number-of-servers-estimation-for-multi-worker-architecture-1" id="number-of-servers-estimation-for-multi-worker-architecture-1"></a>

Let’s calculate the number of servers required to finish crawling in one day. Assume that there is only one worker per server.

��. �� ���� �������� �� 1 ������ �� �������� �ℎ� ����=9.5 �����×365 ����≈3468 ����No. of days required by 1 server to complete the task=9.5 years×365 days≈3468 days

One server takes 3,468 days to complete the task.

How many servers would we need to complete this same task in one day?

We would need 3,468 servers to complete the same task in just one day.

The number of servers required for the web crawler systemCase for multi-threaded server!

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

Since we want to process 10.35PB of data per day the total bandwidth required would be:

10.35��86400���≈120��/���≈960��/���86400sec10.35PB​≈120GB/sec≈960Gb/sec

960��/���960Gb/sec is the total required bandwidth. Now, assume that the task is distributed equally among 3468 �������3468 servers to accomplish the task in one day. Thus, the per server bandwidth would be:

960��/���3468 ������≈277��/��� ��� ������3468 server960Gb/sec​≈277Mb/sec per server

The total bandwidth required for the web crawler system

Let's play around with the initial assumptions and see how the estimates change in the following calculator:

**Estimates Calculator for the Web Crawler**

| Number of Webpages                               | 5       | Billion |
| ------------------------------------------------ | ------- | ------- |
| Text Content per Webpage                         | 2070    | KB      |
| Metadata per Webpage                             | 500     | Bytes   |
| Total Storage                                    | f10.35  | PB      |
| Total Traversal Time on One Server               | f9.5    | Years   |
| Servers Required to Perform Traversal in One Day | f3468   | Servers |
| Bandwidth Estimate                               | f958.33 | Gb/sec  |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

Here is the list of the main building blocks we’ll use in our design:

![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDQ4IiBoZWlnaHQ9IjExMCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=)Building blocks in high-level design

* [**Scheduler**](../distributed-task-scheduler/system-design-the-distributed-task-scheduler.md) is used to schedule crawling events on the URLs that are stored in its database.
* [**DNS**](../domain-name-system/introduction-to-domain-name-system-dns.md) is needed to get the IP address resolution of the web pages.
* [**Cache**](../distributed-cache/system-design-the-distributed-cache.md) is utilized in storing fetched documents for quick access by all the processing modules.
* [**Blob store**](../blob-store/system-design-a-blob-store.md)’s main application is to store the crawled content.

Besides these basic building blocks, our design includes some additional components as well:

* The **HTML fetcher** establishes a network communication connection between the crawler and the web hosts.
* The **service host** manages the crawling operation among the workers.
* The **extractor** extracts the embedded URLs and the document from the web page.
* The **duplicate eliminator** performs dedup testing on the incoming URLs and the documents.

The components in a high-level design

In the next lesson, we’ll focus on the high-level and detailed design of a web crawler.
