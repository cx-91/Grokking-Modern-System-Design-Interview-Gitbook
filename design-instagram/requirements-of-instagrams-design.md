# Requirements of Instagram’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

We’ll concentrate on some important features of Instagram to make this design simple. Let’s list down the requirements for our system:

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

* **Post photos and videos**: The users can post photos and videos on Instagram.
* **Follow and unfollow users**: The users can follow and unfollow other users on Instagram.
* **Like or dislike posts**: The users can like or dislike posts of the accounts they follow.
* **Search photos and videos**: The users can search photos and videos based on captions and location.
* **Generate news feed**: The users can view the news feed consisting of the photos and videos (in chronological order) from all the users they follow. Users can also view suggested and promoted photos in their news feed.

#### Non-functional requirements <a href="#non-functional-requirements-2" id="non-functional-requirements-2"></a>

* **Scalability**: The system should be scalable to handle millions of users in terms of computational resources and storage.
* **Latency**: The latency to generate a news feed should be low.
* **Availability**: The system should be highly available.
* **Durability** Any uploaded content (photos and videos) should never get lost.
* **Consistency**: We can compromise a little on consistency. It is acceptable if the content (photos or videos) takes time to show in followers’ feeds located in a distant region.
* **Reliability**: The system must be able to tolerate hardware and software failures.

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

Our system is read-heavy because service users spend substantially more time browsing the feeds of others than creating and posting new content. Our focus will be to design a system that can fetch the photos and videos on time. There is no restriction on the number of photos or videos that users can upload, meaning efficient storage management should be a primary consideration in the design of this system. Instagram supports around a billion users globally who share 95 million photos and videos on Instagram per day. We’ll calculate the resources and design our system based on the requirements.

Let’s assume the following:

* We have 1 billion users, with 500 million as daily active users.
* Assume 60 million photos and 35 million videos are shared on Instagram per day.
* We can consider 3 MB as the maximum size of each photo and 150 MB as the maximum size of each video uploaded on Instagram.
* On average, each user sends 20 requests (of any type) per day to our service.

#### Storage estimation <a href="#storage-estimation-1" id="storage-estimation-1"></a>

We need to estimate the storage capacity, bandwidth, and the number of servers to support such an enormous number of users and content.

The storage per day will be:

60 million photos/day \* 3 MB = 180 TeraBytes / day

35 million videos/day \* 150 MB = 5250 TB / day

Total content size = 180 + 5250 = 5430 TB

The total space required for a year:

5430 TB/day \* 365 (days a year) = 1981950 TB = 1981.95 PetaBytes

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.41.32 AM.png" alt=""><figcaption></figcaption></figure>

Besides photos and videos, we have ignored comments, status sharing data, and so on. Moreover, we also have to store users’ information and post metadata, for example, userID, photo, and so on. So, to be precise, we need more than 5430 TB/day, but to keep our design simple, let’s stick to the 5430 TB/day.

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

According to the estimate of our storage capacity, our service will get 5430 TB of data each day, which will give us:

5430 TB/(24 \* 60\* 60) = 5430 TB/86400 sec \~= 62.84 GB/s \~= 502.8 Gbps

Since each incoming photo and video needs to reach the users’ followers, let’s say the ratio of readers to writers is 100:1. As a result, we need 100 times more bandwidth than incoming bandwidth. Let’s assume the following:

Incoming bandwidth \~= 502.8 Gbps

Required outgoing bandwidth \~= 100 \* 502.8 Gbps \~= 50.28 Tbps

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.42.13 AM.png" alt=""><figcaption></figcaption></figure>

Outgoing bandwidth is fairly high. We can use compression to reduce the media size substantially. Moreover, we’ll place content close to the users via CDN and other caches in IXP and ISPs to serve the content at high speed and low latency.

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

We need to handle concurrent requests coming from 500 million daily active users. Let’s assume that a typical Instagram server handles 100 requests per second:

Requests by each user per day = 20 Queries handled by a server per second = 100

Queries handled by a server per day = 100 \* 60 \* 60 \* 24 = 8640000

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.42.42 AM.png" alt=""><figcaption></figcaption></figure>

### Try it yourself <a href="#try-it-yourself-0" id="try-it-yourself-0"></a>

Let’s analyze how the number of photos per day affects the storage and bandwidth requirements. For this purpose, try to change values in the following table to compute the estimates. We have set 20 requests by each user per day in the following calculation:



| Requirements                                   | Calculations |
| ---------------------------------------------- | ------------ |
| Number of photos per day (in millions)         | 60           |
| Size of each photo (in MB)                     | 3            |
| Number of videos per day (in millions)         | 35           |
| Size of each video (in MB)                     | 150          |
| Number of daily active users (in millions)     | 500          |
| Number of requests a server can handle per day | 8640000      |
| Storage estimation per day (in TB)             | f5430        |
| Incoming bandwidth (Gb/s)                      | f502.8       |
| Number of servers needed                       | f1157        |

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

In the next lesson, we’ll focus on the high-level design of Instagram. The design will utilize many building blocks that have been discussed in the initial chapters also. We’ll use the following building blocks in our design:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 12.43.08 AM.png" alt=""><figcaption></figcaption></figure>

* A [**load balancer**](../load-balancers/introduction-to-load-balancers.md) at various layers will ensure smooth requests distribution among available servers.
* A [**database**](../databases/introduction-to-databases.md) is used to store the user and accounts metadata and relationship among them.
* [**Blob storage**](../blob-store/system-design-a-blob-store.md) is needed to store the various types of content such as photos, videos, and so on.
* A [**task scheduler**](../distributed-task-scheduler/system-design-the-distributed-task-scheduler.md) schedules the events on the database such as removing the entries whose time to live exceeds the limit.
* A [**cache**](../distributed-cache/system-design-the-distributed-cache.md) stores the most frequent content related requests.
* [**CDN**](../content-delivery-network-cdn/system-design-the-content-delivery-network-cdn.md) is used to effectively deliver content to end users which reduces delay and burden on end-servers.

In the next lesson, we’ll discuss the high-level design of the Instagram system.
