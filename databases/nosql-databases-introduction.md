# NoSQL Databases - Introduction

### What is a NoSQL database? <a href="#what-is-a-nosql-database" id="what-is-a-nosql-database"></a>

As the name implies, NoSQL databases have no SQL; they are more like JSON-based databases built for Web 2.0

NoSQL databases are built for high-frequency read writes, typically required in social applications like micro-blogging, real-time sports apps, online massive multiplayer games, and so on.

### How is a NoSQL database different from a relational database? <a href="#how-is-a-nosql-database-different-from-a-relational-database" id="how-is-a-nosql-database-different-from-a-relational-database"></a>

One obvious question that would pop up in our minds is: why the need for NoSQL databases when relational databases were doing fine, battle-tested, well adopted by the industry, and had no major persistence issues?

Let’s understand the need for NoSQL databases.

### Scalability <a href="#scalability" id="scalability"></a>

Well, one big limitation with SQL-based relational databases is _scalability_. Scaling relational databases is not trivial. They have to be _sharded, replicated_ to make them run smoothly on a cluster. This requires careful planning, human intervention and a skillset.

On the contrary, NoSQL databases can add new server nodes on the fly and scale without any human intervention, just with a snap of your fingers.

Today’s websites need fast read-writes. There are billions of users connected with each other on social networks. A massive amount of data is generated every microsecond, and we need an infrastructure designed to manage this exponential growth.

I’ve dug deep into this, why relational databases can’t scale on the fly, can they be horizontally scaled, like NoSQL databases? If yes, how? How can NoSQL databases horizontally scale, can they support ACID and much more in my [systems design course](https://zerotosoftwarearchitect.com/design-modern-web-scale-distributed-applications-like-a-pro). Check it out.

### Ability to run on clusters <a href="#ability-to-run-on-clusters" id="ability-to-run-on-clusters"></a>

NoSQL databases are designed to run intelligently on clusters. When I say intelligently, I mean with minimal human intervention.

Today, the server nodes even have self-healing capabilities. The infrastructure is intelligent enough to self-recover from faults. This makes things pretty smooth.

However, all this innovation does not mean old-school relational databases aren’t good enough, and we don’t need them anymore.

Relational databases still work like a charm and are still in demand. They have a specific use case. We have already gone through those in the previous lesson.

Also, NoSQL databases had to sacrifice strong consistency, ACID transactions, and much more to scale horizontally over a cluster and across the data centers.

The data with NoSQL databases is more _eventually consistent_ as opposed to being _strongly consistent_.

So, this naturally means NoSQL databases aren’t a silver bullet. They, too, have a use case. And this is completely fine. We don’t need silver bullets. We aren’t hunting werewolves. We are up to a much harder task connecting the world online.

I’ll talk about the underlying design of NoSQL databases in much more detail and why they have to sacrifice strong consistency and transactions in the upcoming lessons.

For now, let’s focus on some of the features of NoSQL databases in the lesson up next.
