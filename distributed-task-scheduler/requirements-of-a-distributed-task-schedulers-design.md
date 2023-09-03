# Requirements of a Distributed Task Scheduler's Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Let’s start by understanding the functional and non-functional requirements for designing a task scheduler.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

The functional requirements of the distributed task scheduler are as follows:

* **Submit tasks**: The system should allow the users to submit their tasks for execution.
* **Allocate resources**: The system should be able to allocate the required resources to each task.
* **Remove tasks**: The system should allow the users to cancel the submitted tasks.
* **Monitor task execution**: The task execution should be adequately monitored and rescheduled if the task fails to execute.
* **Efficient resource utilization**: The resources (CPU and memory) must be used efficiently in terms of time, cost, and fairness. Efficiency means that we do not waste resources. For example, if we allocate a heavy resource to a light task that can easily be executed on a cheap resource, it means that we have not efficiently utilized our resources. Fairness is all tenants’ ability to get the resources with equally likely probability in a certain cost class.
* **Release resources**: After successfully executing a task, the system should take back the resources assigned to the task.
* **Show task status**: The system should show the users the current status of the task.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.45.38 AM.png" alt=""><figcaption></figcaption></figure>

#### Non-functional requirements <a href="#non-functional-requirements-0" id="non-functional-requirements-0"></a>

The non-functional requirements of the distributed task scheduler are as follows:

* **Availability:** The system should be highly available to schedule and execute tasks.
* **Durability:** The tasks received by the system should be durable and should not be lost.
* **Scalability:** The system should be able to schedule and execute an ever-increasing number of tasks per day. Fault-tolerance: The system must be fault-tolerant by providing services uninterrupted despite faults in one or more of its components.
* **Bounded waiting time:** This is how long a task needs to wait before starting execution. We must not execute tasks much later than expected. Users shouldn’t be kept on waiting for an infinite time. If the waiting time for users crosses a certain threshold, they should be notified.

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.46.08 AM.png" alt=""><figcaption></figcaption></figure>

So far in this lesson, we have learned about task schedulers in general and distinguished between centralized and distributed task schedulers. Lastly, we listed the requirements of the distributed task scheduler system.

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

We’ll utilize the following building blocks in the design of our task scheduling system:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 2.46.35 AM.png" alt=""><figcaption></figcaption></figure>

* [**Rate limiter**](https://www.educative.io/collection/page/10370001/4941429335392256/4770834422169600) is required to limit the number of tasks so that our system is reliable.
* [**A sequencer**](https://www.educative.io/collection/page/10370001/4941429335392256/6499939719053312) is needed to uniquely identify tasks.
* [**Database(s)**](https://www.educative.io/collection/page/10370001/4941429335392256/4901035478351872) are used to store task information.
* [**A distributed queue**](https://www.educative.io/collection/page/10370001/4941429335392256/5148400467312640) is required to arrange tasks in the order of execution.
* [**Monitoring**](https://www.educative.io/collection/page/10370001/4941429335392256/6310983387840512) is essential to check the health of the resources and to detect failed tasks to provide reliable service to the users.

We’ve identified the requirements of the task scheduler. In the next lesson, we’ll design our task scheduling system according to these requirements.
