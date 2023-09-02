# Requirements of Quora's Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Let’s understand the functional and non-functional requirements below:

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

A user should be able to perform the following functionalities:

* **Questions and answers**: Users can ask questions and give answers. Questions and answers can include images and videos.
* **Upvote/downvote and comment**: It is possible for users to upvote, downvote, and comment on answers.
* **Search**: Users should have a search feature to find questions already asked on the platform by other users.
* **Recommendation system**: A user can view their feed, which includes topics they’re interested in. The feed can also include questions that need answers or answers that interest the reader. The system should facilitate user discovery with a recommender system.
* **Ranking answers**: We enhance user experience by ranking answers according to their usefulness. The most helpful answer will be ranked highest and listed at the top.

#### Non-functional requirements <a href="#non-functional-requirements-2" id="non-functional-requirements-2"></a>

* **Scalability**: The system should scale well as the number of features and users grow with time. It means that the performance and usability should not be impacted by an increasing number of users.
* **Consistency**: The design should ensure that different users’ views of the same content should be consistent. In particular, critical content like questions and answers should be the same for any collection of viewers. However, it is not necessary that all users of Quora see a newly posted question, answer, or comment right away.
* **Availability**: The system should have high availability. This applies to cases where servers receive a large number of concurrent requests.
* **Performance**: The system should provide a smooth experience to the user without a noticeable delay.

Functional and non-functional requirements of Quora

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

In this section, we’ll make an estimate about the resource requirements for Quora service. We’ll make assumptions to get a practical and tractable estimate. We’ll estimate the number of servers, the storage, and the bandwidth required to facilitate a large number of users.

**Assumptions**: It is important to base our estimation on some underlying assumptions. We, therefore, assume the following:

* There are a total of 1 billion users, out of which 300 million are daily active users.
* Assume 15% of questions have an image, and 5% of questions have a video embedded in them. A question cannot have both at the same time.
* We’ll assume an image is estimated to be 250 KBs, and a video is considered 5 MBs.

#### Number of servers estimation <a href="#number-of-servers-estimation-1" id="number-of-servers-estimation-1"></a>

Let’s estimate our requests per second (RPS) for our design. If there are an average of 300 million daily active users and each user can generate 20 requests per day, then the total number of requests in a day will be:

300×106×20=6×109300×106×20=6×109

Therefore, the RPS = 6×10986400≈69500864006×109​≈69500 approximately requests per second.

**Estimating RPS**

| Daily active users        | 300    | million |
| ------------------------- | ------ | ------- |
| Requests per day per user | 20     |         |
| Requests Per Second (RPS) | f69444 |         |

We already established in the [back-of-the-envelope calculations](https://www.educative.io/collection/page/10370001/4941429335392256/5711642666467328) chapter that we’ll use the following formula to estimate a pragmatic number of servers:

������ �� ����� ������ �������� �� � ������=300×1068000=37500RPS of a serverNumber of daily active users​=8000300×106​=37500

The estimated number of servers required for Quora

> Therefore, the total number of servers required to facilitate 300 million users generating an average of 69,500 requests per second will be 37,500.

#### Storage estimation <a href="#storage-estimation-0" id="storage-estimation-0"></a>

Let’s keep in mind our assumption that 15% of questions have images and 5% have videos. So, we’ll make the following assumptions to estimate the storage requirements for our design:

* Each of the 300 million active users posts 1 question in a day, and each question has 2 responses on average, 10 upvotes, and 5 comments in total.
* The collective storage required for the textual content (including the question, answer(s), and comment(s) text) of one question equals 100 ��100 KB.

**Storage Requirements Estimation Calculator**

| Questions per user                   | 1      | per day  |
| ------------------------------------ | ------ | -------- |
| Total questions per day              | f300   | millions |
| Size of textual content per question | 100    | KB       |
| Image size                           | 250    | KB       |
| Video size                           | 5      | MB       |
| Questions containing images          | 15     | percent  |
| Questions containing videos          | 5      | percent  |
| Storage for textual content          | f30    | TB       |
| Storage for image content            | f11.25 | TB       |
| Storage for video content            | f75    | TB       |

See Detailed Calculations![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNzEyIiBoZWlnaHQ9IjE0MSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=)Summarizing storage requirements of Quora

> Total storage required for one day = 30 ��+11.25 ��+75 �� =116.25 ��30 TB+11.25 TB+75 TB =116.25 TB per day

The daily storage requirements of Quora seem very high. But for service with 300 million DAU, a yearly requirement of 116.25 ��×365=42.43 ��116.25 TB×365=42.43 PB is feasible. The practical requirement will be much higher because we have disregarded the storage required for a number of things. For example, non-active (out of 1 �1 B) users’ data will require storage.

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

The bandwidth estimate requires the calculation of incoming and outgoing data through the network.

* **Incoming traffic**: The incoming traffic bandwidth required per day will be equal to 116.25 ��86400×8=10.9 ����≈11 ����86400116.25 TB​×8=10.9 Gbps≈11 Gbps
* **Outgoing traffic**: We have assumed that 300 million active users views 20 questions per day, so the total bandwidth requirements can be found in the below calculator:

**Bandwidth Requirements Estimation Calculator**

| Total storage required per day      | 116.25  | TB         |
| ----------------------------------- | ------- | ---------- |
| Incoming traffic bandwidth          | f11     | Gbps       |
| Questions viewed per user           | 20      | per day    |
| Total questions viewed              | f69444  | per second |
| Bandwidth for text of all questions | f55.56  | Gbps       |
| Bandwidth for 15% of image content  | f20.83  | Gbps       |
| Bandwidth for 5% of video content   | f138.89 | Gbps       |
| Outgoing traffic bandwidth          | f215.3  | Gbps       |

Detailed Calculations![](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iOTQxIiBoZWlnaHQ9IjE4NCIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiB2ZXJzaW9uPSIxLjEiLz4=)Summarizing the bandwidth requirements of Quora

Total bandwidth requirement of Quora is equal to:

> Incoming + outgoing traffic bandwidth =11 ����+215.3 ����=226.3 ����=11 Gbps+215.3 Gbps=226.3 Gbps

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

We’ll use the following building blocks for the initial design of Quora:

Building blocks required for our design

* [**Load balancers**](https://www.educative.io/collection/page/10370001/4941429335392256/4521972679049216) will be used to divide the traffic load among the service hosts.
* [**Databases**](https://www.educative.io/collection/page/10370001/4941429335392256/4901035478351872) are essential for storing all sorts of data, such as user questions and answers, comments, and likes and dislikes. Also, user data will be stored in the databases. We may use different types of databases to store different data.
* [**A distributed caching system**](https://www.educative.io/collection/page/10370001/4941429335392256/5053577315221504) will be used to store frequently accessed data. We can also use caching to store our view counters for different questions.
* [**The blob store**](https://www.educative.io/collection/page/10370001/4941429335392256/4862646238576640) will keep images and video files.
