# Requirements of a Newsfeed System’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

To limit the scope of the problem, we’ll focus on the following functional and non-functional requirements:

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

* **Newsfeed generation:** The system will generate newsfeeds based on pages, groups, and followers that a user follows. A user may have many friends and followers. Therefore, the system should be capable of generating feeds from all friends and followers. The challenge here is that there is potentially a huge amount of content. Our system needs to decide which content to pick for the user and rank it further to decide which to show first.
* **Newsfeed contents:** The newsfeed may contain text, images, and videos.
* **Newsfeed display:** The system should affix new incoming posts to the newsfeed for all active users based on some ranking mechanism. Once ranked, we show content to a user with higher-ranked first.

#### Non-functional requirements <a href="#non-functional-requirements-2" id="non-functional-requirements-2"></a>

* **Scalability:** Our proposed system should be highly scalable to support the ever-increasing number of users on any platform, such as Twitter, Facebook, and Instagram.
* **Fault tolerance:** As the system should be handling a large amount of data; therefore, partition tolerance (system availability in the events of network failure between the system’s components) is necessary.
* **Availability:** The service must be highly available to keep the users engaged with the platform. The system can compromise strong consistency for availability and fault tolerance, according to the PACELC theorem.
* **Low latency:** The system should provide newsfeeds in real-time. Hence, the maximum latency should not be greater than 2 seconds.

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

Let’s assume the platform for which the newsfeed system is designed has 1 billion users per day, out of which, on average, 500 million are daily active users. Also, each user has 300 friends and follows 250 pages on average. Based on the assumed statistics, let’s look at the traffic, storage, and servers estimation.

#### Traffic estimation <a href="#traffic-estimation-0" id="traffic-estimation-0"></a>

Let’s assume that each daily active user opens the application (or social media page) 10 times a day. The total number of requests per day would be:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.13.51 AM.png" alt=""><figcaption></figcaption></figure>

#### Storage estimation <a href="#storage-estimation-0" id="storage-estimation-0"></a>

Let’s assume that the feed will be generated offline and rendered upon a request. Also, we’ll precompute the top 200 posts for each user. Let’s calculate storage estimates for users’ metadata, posts containing text, and media content.

1.  **Users’ metadata storage estimation:** Suppose the storage required for one user’s metadata is 50 KB. For 1 billion users, we would need 1B×50KB=50TB.

    We can tweak the estimated numbers and calculate the storage for our desired numbers in the following calculator:

**Storage Estimation for the Users' Metadata.**

| Number of users (in billion)                      | 1   |
| ------------------------------------------------- | --- |
| Required storage for one users' metadata (in KBs) | 50  |
| Total storage required for all users (in TBs)     | f50 |

2.  **Textual post’s storage estimation:** All posts could contain some text, we assume it’s 50KB on average. The storage estimation for the top 200 posts for 500 million users would be:

    200×500M×50KB=5PB

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.15.01 AM.png" alt=""><figcaption></figcaption></figure>

**Storage Estimation of Posts Containing Text and Media Content.**

| Number of active users (in million)                            | 500 |
| -------------------------------------------------------------- | --- |
| Maximum allowed text storage per post (in KBs)                 | 50  |
| Number of precomputed posts per user (top N)                   | 200 |
| Storage required for textual posts (in PBs)                    | f5  |
| Total required media content storage for active users (in PBs) | f56 |

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

Considering the above traffic and storage estimation, let’s estimate the required number of servers for smooth operations. Recall that a single typical server can serve 8000 requests per second (RPS). Since our system will have approximately 500 million daily active users (DAU). Therefore, according to estimation in [Back-of-the-Envelope Calculations](../back-of-the-envelope-calculations/page-2.md) chapter, the number of servers we would require is:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.15.52 AM.png" alt=""><figcaption></figcaption></figure>

**Servers Estimation**

| Number of active users (in million) | 500    |
| ----------------------------------- | ------ |
| RPS of a server                     | 8000   |
| Number of servers required          | f62500 |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of newsfeed system utilizes the following building blocks:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.16.14 AM.png" alt=""><figcaption></figcaption></figure>

* [**Database(s)**](../databases/introduction-to-databases.md) is required to store the posts from different entities and the generated personalized newsfeed. It is also used to store users’ metadata and their relationships with other entities, such as friends and followers.
* [**Cache**](../distributed-cache/system-design-the-distributed-cache.md) is an important building block to keep the frequently accessed data, whether posts and newsfeeds or users’ metadata.
* [**Blob storage**](../blob-store/system-design-a-blob-store.md) is essential to store media content, for example, images and videos.
* [**CDN**](../content-delivery-network-cdn/system-design-the-content-delivery-network-cdn.md) effectively delivers content to end-users reducing delay and burden on back-end servers.
* [**Load balancers**](../load-balancers/introduction-to-load-balancers.md) are necessary to distribute millions of incoming clients’ requests for newsfeed among the pool of available servers.

In the next lesson, we’ll focus on the high-level and detailed design of the newsfeed system.
