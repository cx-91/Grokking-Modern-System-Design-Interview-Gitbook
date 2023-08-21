# Time Series Database

### What is a time-series database?[#](https://www.educative.io/module/lesson/web-application-architecture-101/qV81P1Erjmk#What-is-a-time-series-database?) <a href="#what-is-a-time-series-database" id="what-is-a-time-series-database"></a>

Time-series databases are optimized for tracking and persisting data that is continually read and written in the system over a period of time.

### What is time-series data? <a href="#what-is-time-series-data" id="what-is-time-series-data"></a>

It is the data containing data points associated with the occurrence of events with respect to time. These data points are tracked, monitored, and aggregated based on specific business logic.

Time-series data is generally ingested from IoT devices, self-driving vehicles, industry sensors, social networks, stock market financial data, etc.

What is the need for storing such massive amounts of time-series data?

### Why store time-series data? <a href="#why-store-time-series-data" id="why-store-time-series-data"></a>

Studying data streaming-in from applications helps us track the behavior of the system as a whole. It allows us to study user patterns, anomalies, and how things change over time.

Time-series data is primarily used for running analytics and deducing conclusions. It helps the stakeholders make future business decisions by looking at the analytics results. Running analytics enables us to evolve our product continually.

Regular databases are not built to handle time-series data. With the advent of IoT, these databases are getting pretty popular and adopted by the big guns in the industry.

### Popular time-series databases <a href="#popular-time-series-databases" id="popular-time-series-databases"></a>

Some of the popular time-series databases used in the industry are Influx DB, Timescale DB, Prometheus, etc.

### When to pick a time-series database? <a href="#when-to-pick-a-time-series-database" id="when-to-pick-a-time-series-database"></a>

If you have a use case where you need to manage data in real-time, continually over a long period of time, a time-series database is what you need.

As you know, time-series databases are built to deal with streaming data in real-time. Its typical use cases are fetching data from IoT devices, managing data for running monitoring and analytics, writing an autonomous trading platform that deals with changing stock prices in real-time, etc.

### Real-world implementations <a href="#real-world-implementations" id="real-world-implementations"></a>

Here are some of the real-world implementations of the tech:

* [M3DB (a time-series database) powers time-series metrics](https://www.uber.com/en-IN/blog/m3/) workflows at Uber.
* [Apache Druid, a time-series database](https://www.youtube.com/watch?v=W\_Sp4jo1ACg), powers real-time analytics at Airbnb.
