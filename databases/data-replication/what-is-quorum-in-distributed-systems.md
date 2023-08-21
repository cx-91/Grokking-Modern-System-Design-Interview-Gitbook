# What is quorum in distributed systems?

#### Introduction

Data is copied throughout many servers to acquire high availability and fault tolerance in distributed systems, called **replication**. In copying data to all the replicas, an issue comes up: how to ensure that all the replicas have the same data and all the users can access the same data from all the replicas?

**Synchronous replication**

We can use **synchronous replication**, in which the original node reports success to the user only when it has received acknowledgment from all the replicas. The original database waits until it has received acknowledgments from all replicas, as shown in the illustration below:



**Problem in synchronous replication**

If any one of the replica nodes fails to acknowledge due to a network failure or fault, the original node cannot respond to the client until the failed node acknowledges. This causes an increase in the availability of the write operations.

**Quorum as a solution**

A **quorum** in a distributed system is the minimal number of replicas on which a distributed operation (commit/abort) must be completed before proclaiming the operation’s success.

Let’s suppose we have three replicas of the database. The quorum in such an instance is the least number of machines that take the same action, commit or abort, for a particular transaction to determine the final operation for that transaction. So, in a cluster of three replicas, if two replicas acknowledge, the operation can be committed as two replicas make the majority. A quorum guarantees the required consistency for distributed operations.

**Selecting quorum number**

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-21 at 4.51.18 AM.png" alt=""><figcaption></figcaption></figure>

Similarly, if there are five replicas in a cluster and three of them acknowledge, the operation can be committed as the majority is working. Anything less than three replicas will result in the failure of the cluster.

<figure><img src="../../.gitbook/assets/Screenshot 2023-08-21 at 4.51.54 AM.png" alt=""><figcaption></figcaption></figure>

A quorum can be achieved only when the nodes adhere to the methodology w+r>n where w is the minimum nodes for write operations, r is the minimum nodes for read operations, and n is the total number of nodes in a cluster.

In a 3-node cluster, if we read from two nodes, at least one of them will be online and have the updated version, and our cluster will be able to continue functioning. When we have a total of n nodes, all the write operations must be successful in at least w nodes to be regarded as a success for the cluster, and the read operation must be performed from r nodes. We will obtain an updated value from nodes as long as w+r>n since at least one of the nodes we are reading has an updated write. The quorum reads and writes are those that follow these r and w values.
