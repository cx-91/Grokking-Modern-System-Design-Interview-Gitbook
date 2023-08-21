# CAP Theorem

### What is the CAP theorem? <a href="#what-is-the-cap-theorem" id="what-is-the-cap-theorem"></a>

_CAP_ stands for _consistency, availability, partition tolerance_. We’ve gone through _consistency_ and _availability_ in great detail. _Partition tolerance_ means _fault tolerance_, how tolerant the system is of failures or partitions. The system should ideally keep working even when a few nodes go offline.

There are many definitions of this theorem that you will find online. Most of them state that amongst the three, consistency, availability, and partition tolerance, we can only pick two. I find this a teeny tiny bit confusing, so I will try to give a simpler explanation of the theorem.

The CAP theorem states that in case of a network failure, when a few of the system nodes are down, we have to choose between _availability_ and _consistency_.

If we pick availability, this means when a few nodes are down, the other nodes are available to the users to perform write operations updating the application data. In this situation, the system is left inconsistent because the nodes that are down don’t get updated with the new data. When they come back online and if a user fetches the data from them, they’ll return the old values they had when they went down.

If we pick consistency, in this scenario, we have to lock down all the nodes for further writes until the nodes that have gone down come back online. This would ensure the strong consistency of the system because all the nodes will have the same entity values.

Picking between availability and consistency largely depends on our use case and the business requirements. We have been through this in great detail. Also, the design of the distributed systems (CAP theorem) forces us to choose one. We can’t have both availability and consistency at the same time.

Nodes spread around the globe will take some time to reach a consensus. It’s impossible to have zero latency.
