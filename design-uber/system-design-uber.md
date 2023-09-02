# System Design: Uber

### What is Uber? <a href="#what-is-uber-0" id="what-is-uber-0"></a>

**Uber** is an application that provides ride-hailing services to its users. Anyone who needs a ride can register and book a vehicle to travel from source to destination. Anyone who has a vehicle can register as a driver and take riders to their destination. Drivers and riders can communicate through the Uber app on their smartphones.

Car en route to get a rider**1** of 7

The illustration below shows the number of active users of Uber from the start of 2017 to 2020 (source: Statista):

Monthly number of Uber's active users worldwide from 2017 to 2020(by quarter)

### How will we design Uber? <a href="#how-will-we-design-uber-0" id="how-will-we-design-uber-0"></a>

There are many unanswered questions regarding Uber. How does it work? How do drivers connect with riders? These are only two of many. This chapter will design a system like Uber and find the answer to such questions.

We’ve divided the design of Uber into six sections:

1. [**Requirements:**](https://www.educative.io/collection/page/10370001/4941429335392256/6376322179268608) This lesson will describe the functional and non-functional requirements of a system like Uber. We’ll also estimate the requirements of multiple aspects of Uber, such as storage, bandwidth, and the computation resources.
2. [**High-level Design:**](https://www.educative.io/collection/page/10370001/4941429335392256/5429357153091584) We’ll discuss the high-level design of Uber in this lesson. In addition, we’ll also briefly explain the API design of the Uber service.
3. [**Detailed Design:**](https://www.educative.io/collection/page/10370001/4941429335392256/6683135341494272) We’ll explore the detailed design of Uber in this lesson. Moreover, we will also discuss the working of different components used in designing Uber.
4. [**Payment Service and Fraud Detection:**](https://www.educative.io/collection/page/10370001/4941429335392256/6353616188735488) We’ll learn how the payment system works in Uber design. Moreover, we’ll also discuss how we can catch different frauds related to payments in Uber-like systems.
5. [**Evaluation:**](https://www.educative.io/collection/page/10370001/4941429335392256/5469445371199488) This lesson will explain how Uber can fulfill all the non-functional requirements through the proposed design.
6. [**Quiz:**](https://www.educative.io/collection/page/10370001/4941429335392256/6590232799739904) We’ll reinforce major concepts of Uber design via a quiz.

Let’s go over the requirements for designing a system like Uber in the next lesson.
