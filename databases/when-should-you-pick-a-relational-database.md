# When should you pick a relational database?

You should pick a relational database if you need _strong consistency, transactions,_ or _relationships_. Typical examples of apps needing _strong consistency_ are _stock trading, personal banking,_ etc., and relational data is common in apps like _Facebook, LinkedIn,_ etc.

### Transactions and data consistency <a href="#transactions-and-data-consistency" id="transactions-and-data-consistency"></a>

If you are writing software that has anything to do with money or numbers that makes _transactions, ACID_ and _data consistency_ super important to you.

Relational DBs shine when it comes to _transactions_ and _data consistency_. They comply with the ACID rule, have been around for ages, and are battle-tested. More on _strong consistency_ in the upcoming lessons.

### Large community <a href="#large-community" id="large-community"></a>

Additionally, relational databases have a large community. Seasoned engineers on the tech are readily available. You don’t have to go too far looking for them.

### Storing relationships <a href="#storing-relationships" id="storing-relationships"></a>

If your data has a lot of relationships that we typically come across in social networking apps like what friends of yours live in a particular city, which of your friends already ate at the restaurant you plan to visit today, etc. Relational databases suit well for storing this kind of data.

Relational databases are built to store relationships. They have been tried and tested and are used by big guns in the industry. [Facebook leverages a relational database](https://www.scaleyourapp.com/what-database-does-facebook-use-a-1000-feet-deep-dive/) as their main user-facing DB.

### Popular relational databases <a href="#popular-relational-databases" id="popular-relational-databases"></a>

Some of the popular relational databases used in the industry are:

* MySQL, an open-source relationship database written in C and C++, has been around since 1995.
* PostgreSQL, an open-source RDBMS written in C.
* Microsoft SQL Server, a proprietary RDBMS written by Microsoft in C and C++.
* MariaDB, Amazon Aurora, Google Cloud SQL, etc.

Folks!, that’s all on the relational databases. Let’s understand non-relational databases in the lesson up next.
