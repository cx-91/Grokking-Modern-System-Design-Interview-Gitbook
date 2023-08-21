# Features of NoSQL Databases

In the previous lesson, we learned that the NoSQL databases are built to run on clusters in a distributed environment, powering Web 2.0 websites. Now, let’s go over some of the upsides and downsides of using a NoSQL database.

### Pros of NoSQL databases <a href="#pros-of-nosql-databases" id="pros-of-nosql-databases"></a>

Besides their scalable design, NoSQL databases are also developer-friendly. What do I mean by that?

### Learning curve not so steep and schemaless <a href="#learning-curve-not-so-steep-and-schemaless" id="learning-curve-not-so-steep-and-schemaless"></a>

The learning curve of NoSQL databases is less steep than that of relational databases. When working with relational databases, a big chunk of our time goes into learning to design well-normalized tables, setting up relationships, trying to minimize joins, and so on.

Also, one needs to be pretty focused when designing the schema of a relational database to avoid running into issues in the future.

Think of relational databases as a strict headmaster. Everything has to be in place, neat and tidy, and things need to be consistent. However, NoSQL databases are a bit chilled out and relaxed.

There are no strictly enforced schemas. You can work with the data however you want. You can always change stuff and move things around. Entities have no relationships. Thus, there is a lot of flexibility, and you can do things your way.

Wonderful, right?

Not always!! This flexibility is both good and bad at the same time. Being so flexible, developer-friendly, having no joins, relationships, etc. makes it good. But NoSQL databases have limitations too.

Let’s find out what they are.

### Cons Of NoSQL databases <a href="#cons-of-nosql-databases" id="cons-of-nosql-databases"></a>

### Inconsistency <a href="#inconsistency" id="inconsistency"></a>

Since the data is not normalized, this introduces the risk of it being inconsistent. An entity, since spread throughout the database, has to be updated at all places. It’s hard for developers to remember all the locations of an entity in the database; this leads to inconsistency.

Failing to update an entity at all places makes the data inconsistent. This is not a problem with relational databases since they keep the data normalized.

### No support for ACID transactions <a href="#no-support-for-acid-transactions" id="no-support-for-acid-transactions"></a>

Also, NoSQL distributed databases don’t support ACID transactions. A few claim to do so, though they don’t support them at a global deployment level. ACID transactions in these databases are limited to a certain entity hierarchy or a small deployment region where they can lock down nodes to update them.

> **Note:** _ACID transactions in distributed systems come with terms and conditions applied._

I’ve worked on a few NoSQL databases, _MongoDB, Elasticsearch, Google Cloud Datastore_. An upside of working with NoSQL databases is that we don’t have to be a pro in database design to develop an application.

Things are comparatively simple because there is no stress of managing joins, relationships, n+1 query issues and so on. Just fetch the object using its key, which is a constant O(1) operation, making the NoSQL databases fast and simpler.

### Popular NoSQL databases <a href="#popular-nosql-databases" id="popular-nosql-databases"></a>

Some of the popular NoSQL databases used in the industry are _MongoDB, Redis, Neo4J, Cassandra, Memcache,_ etc.

So, by now, we have a pretty good idea of what NoSQL databases are. In the next lesson, let’s look at some of the use cases that best fit them.
