# Document-Oriented Database

### What is a document-oriented database? <a href="#what-is-a-document-oriented-database" id="what-is-a-document-oriented-database"></a>

> _Document-oriented_ databases are the leading types of NoSQL databases. They store data in a document-oriented model in independent documents. The data is generally semi-structured and stored in a JSON-like format.

The data model is similar to the data model of our application code, so it’s easier to store and query data for developers.

Document-oriented stores go along well with the Agile software development methodology as it is easier to change things with evolving demands when working with them. Though datastores/technologies and software development methodologies do not have any co-relation per se.

### Popular document-oriented databases <a href="#popular-document-oriented-databases" id="popular-document-oriented-databases"></a>

Some of the popular document-oriented stores used in the industry are MongoDB, CouchDB, OrientDB, Google Cloud Datastore, and Amazon DocumentDB.

### When do I pick a document-oriented data store for my project? <a href="#when-do-i-pick-a-document-oriented-data-store-for-my-project" id="when-do-i-pick-a-document-oriented-data-store-for-my-project"></a>

Pick a document-oriented data store if:

* You are working with semi-structured data and need a flexible schema that will change often.
* You aren’t sure about the database schema when you start writing the app, there is a possibility that things might change over time and you need something flexible that you could change over time with minimum fuss.
* You need to scale fast and stay highly available.

Typical use cases of document-oriented databases include:

* Real-time feeds
* Live sports apps
* Writing product catalogues
* Inventory management
* Storing user comments
* Web-based multiplayer games

Being in the family of NoSQL databases, document-oriented databases provide horizontal scalability and performant read-writes because they cater to _CRUD_ (Create Read Update Delete) use cases. These include scenarios where there isn’t much complex relational logic involved and all we need is quick persistence and data retrieval.

### Real-world implementations <a href="#real-world-implementations" id="real-world-implementations"></a>

Here are some of the good real-world implementations of the tech:

[SEGA uses Mongo-DB](https://www.mongodb.com/blog/post/sega-hardlight-migrates-to-mongodb-atlas-simplify-ops-improve-experience-mobile-gamers) to improve the experience for millions of mobile gamers.

[Wix Engineering uses MongoDB](https://www.wix.engineering/post/how-we-re-able-to-host-1-million-sites-per-mongodb-cluster) to host sites built on their platform.
