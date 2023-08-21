# Is NoSQL More Performant Than SQL?

Is NoSQL more performant than SQL? Developers ask this question all the time when trying to decide between an SQL and a NoSQL database. And I have a one-word answer for this.

No!!

From a technology performance benchmarking standpoint, both relational and non-relational databases are equally performant.

More than the technology, it’s how we design our systems using a certain technology that decides the performance.

Both SQL and NoSQL tech have their use cases. We have already gone through them in the former lessons. So, don’t get caught up in the hype. Understand your use case and pick the fitting technology.

### Why do popular tech stacks always pick NoSQL databases? <a href="#why-do-popular-tech-stacks-always-pick-nosql-databases" id="why-do-popular-tech-stacks-always-pick-nosql-databases"></a>

Picking the technology based on the use case makes sense, but why do the popular tech stacks always prefer NoSQL databases? For instance, look at the MEAN (_MongoDB, ExpressJS, AngularJS/ReactJS, NodeJS_) stack.

Well, most of the online applications have standard use cases, and these tech stacks have them covered. There are also commercial reasons behind this.

There are a plethora of tutorials available online and a mass promotion of popular tech stacks. With these resources available, it’s easy for beginners to pick them up and write their applications as opposed to researching technologies fitting their use case.

We don’t always need to pick the popular stacks. Rather, we should pick what fits best with our use case.

This course has a separate lesson on how to pick the right tech stack for our app. We will continue this discussion there.

Coming back to performance, as opposed to technology, the performance of an application is more dependent on factors like _application architecture, database design, bottlenecks, network latency,_ etc.

If we use multiple joins in a relational database, the response will inevitably take more time. If we remove all the complex relationships and joins, a relational database will be as quick as a NoSQL one.

### Real-world case studies <a href="#real-world-case-studies" id="real-world-case-studies"></a>

[Facebook uses MySQL](https://www.scaleyourapp.com/what-database-does-facebook-use-a-1000-feet-deep-dive/) for storing its social graph of millions of users. Although it did have to change the DB engine and make some tweaks, MySQL fits best with its use case.

Quora uses MySQL pretty efficiently by partitioning the data at the application level. [Here is an interesting read](https://www.quora.com/Why-does-Quora-use-MySQL-as-the-data-store-instead-of-NoSQLs-such-as-Cassandra-MongoDB-or-CouchDB-Are-they-doing-any-JOINs-over-MySQL-Are-there-plans-to-switch-to-another-DB) on it.

> **Note:** A well-designed SQL data store will always be more performant than a not so well-designed NoSQL store.

### Using both SQL and NoSQL databases in an application <a href="#using-both-sql-and-nosql-databases-in-an-application" id="using-both-sql-and-nosql-databases-in-an-application"></a>

You may be wondering, can’t I use both SQL and a NoSQL datastore in my application? What if I have a requirement fitting both?

You can!! Moreover, all the large-scale online services use a mix of both to achieve the desired persistence behavior.

The term for leveraging the power of multiple databases clubbed together is polyglot persistence. We will discuss this in the next lesson.
