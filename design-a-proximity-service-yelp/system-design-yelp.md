# System Design: Yelp

### What is Yelp? <a href="#what-is-yelp-0" id="what-is-yelp-0"></a>

**Yelp** is a one-stop platform for consumers to discover, connect, and transact with local businesses. With it, users can join a waitlist, make a reservation, schedule an appointment, or purchase goods easily. Yelp also provides information, photos, and reviews about local businesses.

The user provides the name of a place or its GPS location, and the system finds places that are nearby. The user can also upload their opinions on this platform in the form of text, pictures, or ratings for a place they visited. Other location-based services include Foursquare and Google Nearby.

Services based on **proximity servers** are helpful in finding nearby attractions such as restaurants, theaters, or recreational sites. Designing such a system is challenging because we have to efficiently find all the possible places in a given radius with minimum latency. This means that we have to narrow down all the locations in the world, which could be in the billions, and only pinpoint the relevant ones.

![](<../.gitbook/assets/Screenshot 2023-09-03 at 12.05.44 PM.png>)

![](<../.gitbook/assets/Screenshot 2023-09-03 at 12.05.59 PM.png>)

![](<../.gitbook/assets/Screenshot 2023-09-03 at 12.06.18 PM.png>)

![](<../.gitbook/assets/Screenshot 2023-09-03 at 12.06.28 PM.png>)

### How will we design Yelp? <a href="#how-will-we-design-yelp-0" id="how-will-we-design-yelp-0"></a>

Here is the breakdown of Yelp’s design:

1. [**Requirements**](requirements-of-yelps-design.md): In this lesson, we define the requirements and estimate the required servers, storage, and bandwidth of our system.
2. [**Design**](design-of-yelp.md):In this lesson, we define the API design, the database schema, the components of our system, and the workflow of Yelp.
3. [**Design considerations**](design-considerations-of-yelp.md): In this lesson, we dive deep into the design of the Yelp system.
4. [**Quiz**](quiz-on-yelps-design.md): In this lesson, we take a quiz to test our knowledge of Yelp design.

Let’s start our design by defining its requirements.
