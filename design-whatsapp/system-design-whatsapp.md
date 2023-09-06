# System Design: WhatsApp

### WhatsApp <a href="#whatsapp-0" id="whatsapp-0"></a>

In today’s technological world, WhatsApp is an important messaging application that connects billions of people around the globe. Many users start their day by reading or sending WhatsApp messages to their friends and family. According to July 2021 estimates, WhatsApp has two billion active users worldwide. Further, an average WhatsApp user spends approximately 19.4 hours per month on the application.

In December 2020, the WhatsApp CEO tweeted that WhatsApp users share more than 100 billion messages per day, an increase of approximately 54% since 2018. The increase in messages sent globally per day is depicted in the following chart:

<figure><img src="../.gitbook/assets/Screenshot 2023-09-06 at 1.41.03 AM.png" alt=""><figcaption></figcaption></figure>

### Design problem <a href="#design-problem-0" id="design-problem-0"></a>

As system designers, we should be aware of the growth rate of users. There are many interesting questions about WhatsApp:

* How is this application designed?
* How does it work?
* What are the different types of components involved in it?
* How does WhatsApp enable billions of users to communicate with each other?
* How does WhatsApp keep all that data secure?

In this chapter, we’ll focus on the high-level and detailed design of the WhatsApp application to answer the above questions.

### How will we design WhatsApp? <a href="#how-will-we-design-whatsapp-0" id="how-will-we-design-whatsapp-0"></a>

We’ve divided the design of WhatsApp messenger into the following five lessons:

1. [**Requirements**](requirements-of-whatsapps-design.md): In this lesson, we’ll identify functional and non-functional requirements. We’ll also discuss resource estimations required for better and smooth operations of the proposed design of WhatsApp.
2. [**High-level Design**](high-level-design-of-whatsapp.md): We’ll focus on the high-level design of our WhatsApp version. We’ll also discuss essential APIs for our WhatsApp design.
3. [**Detailed Design**](detailed-design-of-whatsapp.md): In this lesson, we’ll describe the design of our WhatsApp messenger in detail. Initially, we’ll explain the design of each microservice, including connection with servers, send and receive messages and media content, and group messages. In the end, the design of each microservice is combined into the detailed design of WhatsApp.
4. [**Evaluation**](evaluation-of-whatsapps-design.md): This lesson will explain how our version of WhatsApp fulfills non-functional requirements. We’ll also evaluate some trade-offs of our design.
5. [**Quiz**](quiz-on-whatsapps-design.md): Here, we’ll assess what we’ve learned in this chapter through a quiz.

Let’s start by discussing the requirements of our version of WhatsApp.
