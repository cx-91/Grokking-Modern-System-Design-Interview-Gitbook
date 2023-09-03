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

500�×10=5500M×10=5 billions request per day ≈58�≈58K requests per second.

Traffic estimation for the newsfeed system

#### Storage estimation <a href="#storage-estimation-0" id="storage-estimation-0"></a>

Let’s assume that the feed will be generated offline and rendered upon a request. Also, we’ll precompute the top 200 posts for each user. Let’s calculate storage estimates for users’ metadata, posts containing text, and media content.

1.  **Users’ metadata storage estimation:** Suppose the storage required for one user’s metadata is 50 KB. For 1 billion users, we would need 1�×50��=50��1B×50KB=50TB.

    We can tweak the estimated numbers and calculate the storage for our desired numbers in the following calculator:

**Storage Estimation for the Users' Metadata.**

| Number of users (in billion)                      | 1   |
| ------------------------------------------------- | --- |
| Required storage for one users' metadata (in KBs) | 50  |
| Total storage required for all users (in TBs)     | f50 |

2.  **Textual post’s storage estimation:** All posts could contain some text, we assume it’s 50KB on average. The storage estimation for the top 200 posts for 500 million users would be:

    200×500�×50��=5��200×500M×50KB=5PB
3.  **Media content storage estimate:** Along with text, a post can also contain media content. Therefore, we assume that 1/5�ℎ1/5th posts have videos and 4/5�ℎ4/5th include images. The assumed average image size is 200KB and the video size is 2MB.

    Storage estimate for 200 posts of one user: (200×2��×15)+(200×200��×45)=80��+32��=112��(200×2MB×51​)+(200×200KB×54​)=80MB+32MB=112MB

    Total storage required for 500 million users’ posts: 112��×500�=56��112MB×500M=56PB

    So we’ll need at least 56PB of blob storage to store the media content.

Storage required for 500 million active users per day (each with approx. 200 posts) by newsfeed system

**Storage Estimation of Posts Containing Text and Media Content.**

| Number of active users (in million)                            | 500 |
| -------------------------------------------------------------- | --- |
| Maximum allowed text storage per post (in KBs)                 | 50  |
| Number of precomputed posts per user (top N)                   | 200 |
| Storage required for textual posts (in PBs)                    | f5  |
| Total required media content storage for active users (in PBs) | f56 |

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

Considering the above traffic and storage estimation, let’s estimate the required number of servers for smooth operations. Recall that a single typical server can serve 8000 requests per second (RPS). Since our system will have approximately 500 million daily active users (DAU). Therefore, according to estimation in [Back-of-the-Envelope Calculations](https://www.educative.io/collection/page/10370001/4941429335392256/4766860129599488) chapter, the number of servers we would require is:

������������ServerRPSDAU​ =500�8000=62500=8000500M​=62500 servers.

Number of servers required for the newsfeed system

**Servers Estimation**

| Number of active users (in million) | 500    |
| ----------------------------------- | ------ |
| RPS of a server                     | 8000   |
| Number of servers required          | f62500 |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of newsfeed system utilizes the following building blocks:

The building blocks to design a newsfeed system

* [**Database(s)**](https://www.educative.io/collection/page/10370001/4941429335392256/4901035478351872) is required to store the posts from different entities and the generated personalized newsfeed. It is also used to store users’ metadata and their relationships with other entities, such as friends and followers.
* [**Cache**](https://www.educative.io/collection/page/10370001/4941429335392256/5053577315221504) is an important building block to keep the frequently accessed data, whether posts and newsfeeds or users’ metadata.
* [**Blob storage**](https://www.educative.io/collection/page/10370001/4941429335392256/4862646238576640) is essential to store media content, for example, images and videos.
* [**CDN**](https://www.educative.io/collection/page/10370001/4941429335392256/6624266925899776) effectively delivers content to end-users reducing delay and burden on back-end servers.
* [**Load balancers**](https://www.educative.io/collection/page/10370001/4941429335392256/4521972679049216) are necessary to distribute millions of incoming clients’ requests for newsfeed among the pool of available servers.

In the next lesson, we’ll focus on the high-level and detailed design of the newsfeed system.
