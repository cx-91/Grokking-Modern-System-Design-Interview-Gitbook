---
description: Try your hand at some of the back-of-the-envelope numbers.
---

# Examples of Resource Estimation

### Introduction <a href="#introduction" id="introduction"></a>

Now that we’ve set the foundation for resource estimation, let’s make use of our background knowledge to estimate resources like servers, storage, and bandwidth. Below, we consider a scenario and a service, make assumptions, and based on those assumptions, we make estimations. Let’s jump right in!

### Number of servers required <a href="#number-of-servers-required" id="number-of-servers-required"></a>

Let’s make the following assumptions about a Twitter-like service.

**Assumptions:**

* There are 500 million (M) daily active users (DAU).
* A single user makes 20 requests per day on average.
* Recall that a single server can handle 8,000 RPS.

**Estimating the Number of Servers**

| Daily active users (DAU)  | 500  | M       |
| ------------------------- | ---- | ------- |
| Requests on average / day | 20   |         |
| Total requests / day      | f10  | Billion |
| Total requests / second   | f115 | K       |
| Total servers required    | f15  |         |

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.17.33 AM.png" alt=""><figcaption></figcaption></figure>

Indeed, the number above doesn’t seem right. If we only need 15 commodity servers to serve 500M daily users, then why do big services use millions of servers in a data center? The primary reason for this is that the RPS is not enough to estimate the number of servers required to provide a service. Also, we made some underlying assumptions in the calculations above. One of the assumptions was that a request is handled by one server only. In reality, requests go through to web servers that may interact with application servers that may also request data from storage servers. Each server may take a different amount of time to handle each request. Furthermore, each request may be handled differently depending upon the state of the data center, the application, and the request itself. Remember that we have a variety of servers for providing various services within a data center.

We have established that:

1. Finding accurate capacity estimations is challenging due to many factors—for example, each service being designed differently, having different fan-out rules and hardware, server responsibilities changing over time, etc.
2. At the design level, a coarse-grained estimation is appropriate because the purpose is to come up with reasonable upper bounds on the required resources. We can also use advanced methods from the queuing theory and operations research to make better estimates. However, an interview or initial design is not an appropriate time to use such a strategy. Real systems use various methods (back-of-the-envelope calculations, simulations, prototyping, and monitoring) to improve on their initial (potentially sloppy) estimates gradually.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.17.59 AM.png" alt=""><figcaption></figcaption></figure>

2. We assume that there is a specific second in the day when all the requests of all the users arrive at the service simultaneously. We use it to get the capacity estimation for a peak load. To do better, we will need request and response distributions, which might be available at the prototyping level. We might assume that distributions follow a particular type of distribution, for example, the Poisson distribution. By using DAU as a proxy for peak load for a _specific second_, we have avoided difficulties finding the arrival rates of requests and responses.

Therefore, the DAU will then become the number of requests per second. We already have RPS for a server; therefore, equation (1) becomes:

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.18.31 AM.png" alt=""><figcaption></figcaption></figure>

> **Note:** Our calculations are based on an approximation that might not give us a tight bound on the number of servers, but still it’s a realistic one. Therefore, we use this approach in estimating the number of servers in our design problems. Informally, the equation given above assumes that one server can handle 8,000 users per second. We use this reference in the rest of the course as well.

### Storage requirements <a href="#storage-requirements" id="storage-requirements"></a>

In this section, we attempt to understand how storage estimation is done by using Twitter as an example. We estimate the amount of storage space required by Twitter for new tweets in a year. Let’s make the following assumptions to begin with:

* We have a total of 250 M daily active users.
* Each user posts three tweets in a day.
* Ten percent of the tweets contain images, whereas five percent of the tweets contain a video. Any tweet containing a video will not contain an image and vice versa.
* Assume that an image is 200 KB and a video is 3 MB in size on average.
* The tweet text and its metadata require a total of 250 Bytes of storage in the database.

Then, the following storage space will be required:

**Estimating Storage Requirements**

| Daily active users (DAU)   | 250    | M  |
| -------------------------- | ------ | -- |
| Daily tweets               | 3      |    |
| Total requests / day       | f750   | M  |
| Storage required per tweet | 250    | B  |
| Storage required per image | 200    | KB |
| Storage required per video | 3      | MB |
| Storage for tweets         | f187.5 | GB |
| Storage for images         | f15    | TB |
| Storage for videos         | f112.5 | TB |
| Total storage              | f128   | TB |

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.18.31 AM (1).png" alt=""><figcaption></figcaption></figure>

### Bandwidth requirements <a href="#bandwidth-requirements" id="bandwidth-requirements"></a>

In order to estimate the bandwidth requirements for a service, we use the following steps:

1. Estimate the daily amount of incoming data to the service.
2. Estimate the daily amount of outgoing data from the service.
3. Estimate the bandwidth in Gbps (gigabits per second) by dividing the incoming and outgoing data by the number of seconds in a day.

**Incoming traffic:** Let’s continue from our previous example of Twitter, which requires 128 TBs of storage each day. Therefore, the incoming traffic should support the following bandwidth per second:

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.19.59 AM.png" alt=""><figcaption></figcaption></figure>

> **Note:** We multiply by 8 in order to convert Bytes(B) into bits(b) because bandwidth is measured in bits per second.

**Outgoing traffic:** Assume that a single user views 50 tweets in a day. Considering the same ratio of five percent and 10 percent for videos and images, respectively, for the 50 tweets, 2.5 tweets will contain video content whereas five tweets will contain an image. Considering that there are 250 M active daily users, we come to the following estimations:

**Estimating Bandwidth Requirements**

| Daily active users (DAU)      | 250    | M        |
| ----------------------------- | ------ | -------- |
| Daily tweets viewed           | 50     | per user |
| Tweets viewed / second        | f145   | K        |
| Bandwidth required for tweets | f0.3   | Gbps     |
| Bandwidth required for images | f23.2  | Gbps     |
| Bandwidth required for videos | f174   | Gbps     |
| Total bandwidth               | f197.5 | Gbps     |

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 1.20.39 AM.png" alt=""><figcaption><p>The total bandwidth required by Twitter</p></figcaption></figure>

