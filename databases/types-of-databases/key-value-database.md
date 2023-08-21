# Key-Value Database

### What is a key-value database? <a href="#what-is-a-key-value-database" id="what-is-a-key-value-database"></a>

Key-value databases are also a part of the NoSQL family. These databases use a simple key-value pairing method to store and quickly fetch the data with minimum latency.

### Features of a key-value database <a href="#features-of-a-key-value-database" id="features-of-a-key-value-database"></a>

Due to the minimum latency they ensure, that is constant O(1) time, the primary use case for these databases is caching application data.

The key serves as a unique identifier and has a value associated with it. The value can be as simple as a string and as complex as an object graph.

The data in key-value databases can be fetched in constant time O(1), and there is no query language required to fetch the data. It’s just a simple no-brainer fetch operation. This ensures minimum latency.

As discussed earlier in the course, these databases are also used to achieve a consistent state in distributed systems.

### Popular key-value databases <a href="#popular-key-value-databases" id="popular-key-value-databases"></a>

Some of the popular key-value data stores used in the industry are Redis, Hazelcast, Riak, Voldemort, and Memcached.

### When do I pick a key-value database? <a href="#when-do-i-pick-a-key-value-database" id="when-do-i-pick-a-key-value-database"></a>

If you have a use case where you need to fetch data real fast with minimum fuss, you should pick a key-value datastore.

Key-value stores are built to serve use cases that require super-fast data fetch.

Typical use cases of a key-value database are:

* Caching
* Persisting user state
* Persisting user sessions
* Managing real-time data
* Implementing queues
* Creating leaderboards in online games and web apps
* Implementing a pub-sub system

### Real-world implementations <a href="#real-world-implementations" id="real-world-implementations"></a>

Here are some of the real-world implementations of the tech:

* [Twitter leverages Redis](https://blog.twitter.com/engineering/en\_us/topics/infrastructure/2017/the-infrastructure-behind-twitter-scale) in its infrastructure.
* [Google Cloud uses Memcached](https://cloud.google.com/appengine/docs/standard/python/memcache/) to implement caching on their cloud platform.
