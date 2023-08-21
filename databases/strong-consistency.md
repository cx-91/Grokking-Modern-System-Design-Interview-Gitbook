# Strong Consistency

### What is strong consistency? <a href="#what-is-strong-consistency" id="what-is-strong-consistency"></a>

_Strong consistency_ simply means the data has to be strongly consistent at all times; all the server nodes across the world should contain the same value of an entity at any point in time. The only way to implement this behavior is by locking down the nodes as they are being updated.

### Real-world use case <a href="#real-world-use-case" id="real-world-use-case"></a>

We will continue the same eventual consistency example from the previous lesson. To ensure strong consistency in the system, when the user in Japan likes the post, all the nodes across different geographical zones have to be locked down to prevent any concurrent updates.

This means that, at one point in time, only one user can update the tweet like counter.

<figure><img src="../.gitbook/assets/Screenshot 2023-08-21 at 4.26.21 AM.png" alt=""><figcaption></figcaption></figure>

Once the user in Japan updates the like counter from 100 to 101. The value gets replicated globally across all nodes. Once all the nodes reach a consensus, the locks get lifted.

Now, other users can like the tweet. If the nodes take a while to reach a consensus, they have to wait until then.

Well, this behavior surely is not desirable for social applications, but think of a stock market application where the users see different prices of the same stock at one point in time and update it concurrently. This would create chaos.

To avoid this confusion and chaos, we need our systems to be strongly consistent at all times. The nodes have to be locked down for updates.

Queuing all the write requests is one good way of making a system strongly consistent. The implementation is beyond the scope of this course. However, there is a lesson on this in the message queue chapter.

So, by now, I am sure you have realized that moving ahead with the strong consistency model hits the capability of the system to scale and be highly available. While one user updates the data, the system does not allow other users to perform concurrent updates. However, this behavior is what enables the implementation of ACID transactions. In my distributed systems design course, I’ve dug deep into this.

Distributed systems like NoSQL databases that scale horizontally on the fly don’t support ACID transactions globally, and this is due to their inherent design. Well, the whole reason for the development of NoSQL tech was its ability to scale and be highly available.

This is pretty much it for strong consistency. Now, let’s look into the _CAP theorem_ in the lesson up next.
