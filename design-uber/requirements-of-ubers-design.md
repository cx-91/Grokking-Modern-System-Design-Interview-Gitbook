# Requirements of Uber’s Design

### Requirements <a href="#requirements-0" id="requirements-0"></a>

Let’s start with the requirements for designing a system like Uber.

#### Functional requirements <a href="#functional-requirements-1" id="functional-requirements-1"></a>

The functional requirements of our system are as follows:

* **Update driver location**: The driver is a moving entity, so the driver’s location should be automatically updated at regular intervals.
* **Find nearby drivers**: The system should find and show the nearby available drivers to the rider.
* **Request a ride**: A rider should be able to request a ride, after which the nearest driver should be notified about the rider’s requests.
* **Manage payments:** At the start of the trip, the system must initiate the payment process and manage the payments.
* **Show driver estimated time of arrival (ETA):** The rider should be able to see the estimated time of arrival of the driver.
* **Confirm pickup:** Drivers should be able to confirm that they have picked up the rider.
* **Show trip updates**: Once a driver and a rider accept a ride, they should be able to constantly see trip updates like ETA and current location until the trip finishes.
* **End the trip:** The driver marks the journey complete upon reaching the destination, and they then become available for the next ride.

**Question**

What if two drivers are at the same distance from the rider? How will we select the driver to whom we’ll send the request?

This decision will be made on multiple factors, such as the distance, type of vehicle, rank of the driver, and so on. Still, if two drivers are identical, we can randomly select one and send a request to that driver. If one driver doesn’t accept the ride within a few seconds, we retract the ride offer from this driver and present it to a new one.

\-----------------

#### Non-functional requirements <a href="#non-functional-requirements-0" id="non-functional-requirements-0"></a>

The non-functional requirements of our system are as follows:

* **Availability:** The system should be highly available. The downtime of even a fraction of a second can result in a trip failure, in the driver being unable to locate the rider, or in the rider being unable to contact the driver.
* **Scalability:** The system should be scalable to handle an ever-increasing number of drivers and riders with time.
* **Reliability:** The system should provide fast and error-free services. Ride requests and location updates should happen smoothly.
* **Consistency:** The system must be strongly consistent. The drivers and riders in an area should have a consistent view of the system.
* **Fraud detection:** The system should have the ability to detect any fraudulent activity related to payment.

### Resource estimation <a href="#resource-estimation-0" id="resource-estimation-0"></a>

Now, let’s estimate the resources for our design. Let’s assume it has around 500 million riders and about five million drivers. We’ll assume the following numbers for our estimates:

* We have 20 million daily active riders and three million daily active drivers.
* We have 20 million daily trips.
* All active drivers send a notification of their current location every four seconds.

#### Storage estimation <a href="#storage-estimation-0" id="storage-estimation-0"></a>

Let’s estimate the storage required for our system:

**Rider’s metadata**

Let’s assume we need around 1,000 Bytes to store each rider’s information, including ID, name, email, and so on, when the rider registers in our application. To store the 500 million riders, we require 500 GB of storage:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.01.43 PM.png" alt=""><figcaption></figcaption></figure>

Additionally, if we have around 500,000 new riders registered daily, we’ll need a further 500 MB to store them.

**Driver’s metadata**

Let’s assume we need around 1,000 Bytes to store each driver’s information, including ID, name, email, vehicle type, and so on, when the driver registers in our application. To store the five million drivers, we require 5 GB of storage:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.01.55 PM.png" alt=""><figcaption></figcaption></figure>

Additionally, if we have around 100,00 new drivers registered daily, we’ll need around 100 MB to store them.

**Driver location metadata**

Let’s assume we need around 36 Bytes to store the driver’s location updates. If we have five million drivers, we need around 180 MB of storage just for the drivers’ locations.

**Trip metadata**

Let’s assume we need around 100 Bytes to store single trip information, including trip ID, rider ID, driver ID, and so on. If we have 20 million daily rides, we need around 2 GB of storage for the trip data.

Let’s calculate the total storage required for Uber in a single day:

**Storage Capacity Estimation**

| Number of drivers (millions)                                    | 5     |
| --------------------------------------------------------------- | ----- |
| Storage required to store a driver's location (Bytes)           | 36    |
| Total storage required to store drivers’ locations (MB per day) | f180  |
| Number of trips (millions)                                      | 20    |
| Storage required to store a trip (Bytes)                        | 100   |
| Total storage required to store trips (GB per day)              | f2    |
| Storage required for new riders daily (MB per day)              | 500   |
| Storage required for new drivers daily (MB per day)             | 100   |
| Total storage (GB per day)                                      | f2.78 |

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.02.25 PM.png" alt=""><figcaption></figcaption></figure>

> **Note:** We can adjust the values in the table to see how the estimations change.

#### Bandwidth estimation <a href="#bandwidth-estimation-0" id="bandwidth-estimation-0"></a>

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.02.48 PM.png" alt=""><figcaption></figcaption></figure>

**Bandwidth Requirements**

| Trips per second                                 | 232     |
| ------------------------------------------------ | ------- |
| Bandwidth for each trip (Bytes)                  | 100     |
| Total bandwidth for trips (kilo bits per second) | f185.6  |
| Active drivers (millions)                        | 3       |
| Bandwidth for each driver (Bytes)                | 19      |
| Bandwidth for drivers (Megabits per second)      | f114    |
| Total bandwidth (Megabits per second)            | f114.19 |

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.03.05 PM.png" alt=""><figcaption></figcaption></figure>

> **Note:** We can adjust the values in the table to see how the requirements change.

We’ve ignored the bandwidth from the Uber service to users because it was very small. More bandwidth will be required for sending map data from the Uber service to users, which we’ve also discussed in the [Google Maps](../design-google-maps/system-design-google-maps.md) chapter.

#### Number of servers estimation <a href="#number-of-servers-estimation-0" id="number-of-servers-estimation-0"></a>

We need to handle concurrent requests coming from 20 million daily active users. We’ll use the following formula to estimate a pragmatic number of servers. We established this formula in the [Back-of-the-envelope Calculations](../back-of-the-envelope-calculations/put-back-of-the-envelope-numbers-in-perspective.md) chapter:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.03.49 PM.png" alt=""><figcaption></figcaption></figure>

**Estimating the Number of Servers**

| Daily active users (millions) | 20    |
| ----------------------------- | ----- |
| RPS of a server               | 8000  |
| Number of servers required    | f2500 |

> **Note:** We can adjust the values in the table to see how the estimations change.

### Building blocks we will use <a href="#building-blocks-we-will-use-0" id="building-blocks-we-will-use-0"></a>

The design of Uber utilizes the following building blocks:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-03 at 7.04.04 PM.png" alt=""><figcaption></figcaption></figure>

* [**Databases**](../databases/introduction-to-databases.md) store the metadata of riders, drivers, and trips.
* [**A cache**](../content-delivery-network-cdn/introduction-to-a-cdn.md) stores the most requested data for quick responses.
* [**CDNs**](../content-delivery-network-cdn/introduction-to-a-cdn.md) are used to effectively deliver content to end users, reducing delay and the burden on end-servers.
* [**A load balancer**](../load-balancers/introduction-to-load-balancers.md) distributes the read/write requests among the respective services.

Riders’ and drivers’ devices should have sufficient bandwidth and GPS equipment for smooth navigation with maps.

> **Note:** The information provided in this chapter is inspired by the engineering blog of Uber.
