# Evaluation of Uber’s Design

### Fulfill non-functional requirements <a href="#fulfill-non-functional-requirements-0" id="fulfill-non-functional-requirements-0"></a>

Let’s evaluate how our system fulfills the non-functional requirements.

#### Availability <a href="#availability-1" id="availability-1"></a>

Our system is highly available. We used WebSocket servers. If a user gets disconnected, the session is recreated via a load balancer with a different server. We’ve used multiple replicas of our databases with a primary-secondary replication model. We have the Cassandra database, which provides highly available services and no single point of failure. We used a CDN, cache, and load balancers, which increase the availability of our system.

#### Scalability <a href="#scalability-2" id="scalability-2"></a>

Our system is highly scalable. We used many independent services so that we can scale these services horizontally, independent of each other as per our needs. We used QuadTrees for searching by dividing the map into smaller segments, which shortens our search space. We used a CDN, which increases the capacity to handle more users. We also used a NoSQL database, Cassandra, which is horizontally scalable. Additionally, we used load balancers, which improve speed by distributing read workload among different servers.

#### Reliability <a href="#reliability-3" id="reliability-3"></a>

Our system is highly reliable. The trip can continue even if the rider’s or driver’s connection is broken. This is achieved by using their phones as local storage. The use of multiple WebSocket servers ensures smooth, nearly real-time operations. If any of the servers fail, the user is able to reconnect with another server. We also used redundant copies of the servers and databases to ensure that there’s no single point of failure. Our services are decoupled and isolated, which eventually increases the reliability. Load balancers help move the requests away from any failed servers to healthy ones.

#### Consistency <a href="#consistency-4" id="consistency-4"></a>

We used storage like MySQL to keep our data consistent globally. Moreover, our system does synchronous replication to achieve strong consistency. Because of a limited number of data writers and viewers for a trip (rider, driver, some internal services), the usage of traditional databases doesn’t become a bottleneck. Also, data sharding is easier in this scenario.

#### Fraud detection <a href="#fraud-detection-5" id="fraud-detection-5"></a>

Our system is able to detect any fraudulent activity related to payment. We used the RADAR system to detect any suspicious activity. RADAR recognizes the beginning of a fraud attempt and creates a rule to prevent it.

### Meeting Non-functional Requirements

| **Requirements** | **Techniques**                                                                                                                                             |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Availability     | <ul><li>Using server replicas</li><li>Using database replicas with Cassandra database</li><li>Load balancers hide server failures from end users</li></ul> |
| Scalability      | <ul><li>Horizontal sharding of the database</li><li>The Cassandra NoSQL database</li></ul>                                                                 |
| Reliability      | <ul><li>No single point of failure</li><li>Redundant components</li></ul>                                                                                  |
| Consistency      | <ul><li>Strong consistency using synchronous replications</li></ul>                                                                                        |
| Fraud detection  | <ul><li>Using RADAR to recognize and prevent any fraud related to payments</li></ul>                                                                       |

### Conclusion <a href="#conclusion-0" id="conclusion-0"></a>

This chapter taught us how to design a ride-hailing service like Uber. We discussed its functional and non-functional requirements. We learned how to efficiently locate drivers on the map using QuadTrees. We also discussed how to efficiently calculate the estimated time of arrival using routing algorithms and machine learning. Additionally, we learned that guarding our service against fraudulent activities is important for the business’s success.
