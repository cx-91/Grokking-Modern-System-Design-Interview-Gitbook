# What is a quorum?

Definition

A **quorum** is the minimum number of members that must be present at any meeting to consider the proceedings of the meeting valid.

**Quorum in distributed systems**

Quorum is a widely used concept in distributed systems. For many of the standard algorithms, the idea of quorum is used to reach a decision.

Let’s understand the concept of quorum with the help of a diagram.

![widget](https://www.educative.io/cdn-cgi/image/format=auto,width=3000,quality=75/api/edpresso/shot/6417369123782656/image/4544139094130688)

In Fig A, we have two nodes, each with green and red colors. In this case, there can be no consensus, as a quorum of nodes has not agreed. On the other hand, in Fig B, we have three green nodes, and hence we have a quorum. Whatever these three nodes agree upon is what the algorithm will do.

In distributed systems, we mostly have an odd number of nodes, so the chances of us running into a situation like the one depicted in Fig A are slim. There will always be more than half the number of given nodes deciding on something, provided that there are two options to choose from.

####

**Problem of split brain in distributed systems**

Network partition is an inherent part of any distributed system, and a system that claims to be distributed also needs to be partition tolerant. That is, in the event of a network partition, the system should continue to work.

Quorum usually helps with this continuity in the event of a network partition. In the absence of a quorum, the different partitions would each assume that the view of the system is as they see it, which is limited by the communication. Each of the partitions would then continue to work and move forward in different directions. This problem is known as **split brain**.

With the concept of quorum, the system would only work if more than half the nodes can communicate with each other through some network path.

####

**Conclusion**

Quorum is used in almost all consensus-based algorithms in distributed systems. `Raft` is one of the famous consensus algorithms that uses quorum as the core of their algorithm logic.
