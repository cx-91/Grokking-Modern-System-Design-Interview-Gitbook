# System Design: Twitter

### Twitter <a href="#twitter-0" id="twitter-0"></a>

Twitter is a free microblogging social network where registered users post messages called “Tweets”. Users can also like, reply to, and retweet public tweets. Twitter has about 397 million users as of 2021 that is proliferating with time. One of the main reasons for its popularity is the vast sharing of the breaking news on the platform. Another important reason is that Twitter allows us to engage with and learn from people belonging to different communities and cultures.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.14.12 PM.png" alt=""><figcaption></figcaption></figure>

The illustration below shows the country-wise userbase of Twitter as of January 2022 (source: Statista), where the US is taking the lead. Such statistics are important when we design our infrastructure because it allows us to allocate more capacity to the users of a specific region, and traffic served from near specific users.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.14.27 PM.png" alt=""><figcaption></figcaption></figure>

### How will we design Twitter? <a href="#how-will-we-design-twitter-0" id="how-will-we-design-twitter-0"></a>

We’ll divide Twitter’s design into four sections:

1. [**Requirements**](requirements-of-twitters-design.md): This lesson describes the functional and non-functional requirements of Twitter. We’ll also estimate multiple aspects of Twitter, such as storage, bandwidth, and computational resources.
2. [**Design**](high-level-design-of-twitter.md): We’ll discuss the high-level design of Twitter in this lesson. We also briefly explain the API design and identify the significant components of the Twitter architecture. Moreover, we will discuss how to manage the Top-k problem, such as Tweets liked or viewed by millions of users on Twitter.
3. [**Client-side load balancers**](client-side-load-balancer-for-twitter.md): This lesson discusses how Twitter performs load balancing for its microservices system to manage billions of requests between various services’ instances. Furthermore, we also see why Twitter uses a customized load-balancing technique instead of other commonly used approaches.
4. [**Quiz**](quiz-on-twitters-design.md): Finally, we’ll reinforce major concepts of Twitter design with a quiz.

Let’s begin with defining Twitter’s requirements.
