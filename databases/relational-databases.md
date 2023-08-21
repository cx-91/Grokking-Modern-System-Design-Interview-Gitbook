# Relational Databases

### What is a relational database? <a href="#what-is-a-relational-database" id="what-is-a-relational-database"></a>

A _relational database_ persists data containing relationships: _one to one, one to many, many to many, many to one,_ etc. It is the most widely used type of database in web development.

Relational databases have a relational data model, data is organized in _tables_ having _rows_ and _columns_ and _SQL_ is the primary data query language used to interact with relational databases.

MySQL is an example of a relational database.

### What are relationships? <a href="#what-are-relationships" id="what-are-relationships"></a>

Imagine you buy five different books from an online bookstore. When you create an account at the bookstore, the system will assign you a customer id say C1. Now, C1 will be linked to five different books B1, B2, B3, B4, and B5.

This is a _one-to-many_ relationship. In the simplest of forms, one database table will contain the details of all the customers and another table will contain all the products in the inventory.

One row in the customer table will correspond to multiple rows in the product inventory table.

Upon pulling the user object with the id C1 from the database, we can easily find what books C1 purchased via the relationship model.

### Data consistency <a href="#data-consistency" id="data-consistency"></a>

Besides the relationships, relational databases also ensure saving data in a normalized fashion. In very simple terms, normalized data means an entity occurs in only one place/table in its simplest and atomic form and is not spread throughout the database.

This helps maintain consistency in the data. In the future, if we want to update the data, we update it in just one place as opposed to updating the entity spread through multiple tables. This is troublesome, and things can quickly get inconsistent.

### ACID transactions <a href="#acid-transactions" id="acid-transactions"></a>

Besides _normalization_ and _consistency_, relational databases also ensure _ACID_ transactions.

ACID stands for _atomicity, consistency, isolation_ and _durability_.

An _ACID_ transaction means if a transaction, say a financial transaction, occurs in a system, it will be executed with perfection without affecting any other processes or transactions. After the transaction is complete, the system will have a new state that is _durable_ and _consistent_.

In case anything amiss happens during the transaction, say a minor system failure, the entire operation is rolled back.

An _ACID_ transaction happens with an initial state of the system, _State A_, and completes with a final state of the system, _State B_. Both the states are consistent and durable.

A relational database ensures that the system is either in _State A_ or _State B_ at all times. There is no middle state. If anything fails, the system always rolls back to _State A_.

In the next lesson, let’s understand when to pick a relational database.
