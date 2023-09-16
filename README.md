# Architectural Katas 2023

## Who we are

We are a bunch of enthusiatic developers, designers and product leads with goal to make technology valuable and sustainable. We work at Atria Convergence Technologies Limited in India and Our team consists of Akash Kumar R, Manjuprasad Nidlady, Madhusudhan K, Veni Kota and Koushik Ithal.

## Problem Analysis

### Business Case
Road Warrior brain child of a start up which is  a web and mobile app that allows travelers to see all of their existing reservations organized by trip, either online or through their mobile device. Road Warrior automatically polls your email looking for travel-related emails, and filters and whitelists certain emails to avoid spam and duplicates. Road Warrior also interfaces with the agency’s existing airline, hotel, and car rental interface system to update travel details such as delays, cancellations, updates, gate changes, etc. You can also add, update, or delete existing reservations manually as well.
Road Warrior lets you group your items by trip, and once the trip is complete, the items are automatically removed from the dashboard. You can also share your trip information by interfacing with standard social media sites or allowing targeted people to view your trip. Road Warrior has the richest user interface possible across all deployment platforms, and provides end-of-year summary reports for users with a wide range of metrics about their travel usage.
Road Warrior also gathers analytical data from users trips for various purposes, such as travel trends, locations, airline and hotel vendor preferences, cancellation and update frequency, and so on. Road Warrior integrates seamlessly with existing travel systems such as SABRE and APOLLO, and with your preferred travel agency for quick problem resolution.
Road Warrior works internationally, and supports multiple languages and currencies. Road Warrior is the ultimate solution for travelers who want to simplify their travel management and enjoy their trips without hassle.

### Domain Understanding

•	When planning a trip, travelers increasingly rely on their mobile devices. According to Travelport Digital, 80% of travelers used a mobile app to research trips in 2018 statista.com. Mobile usage has become an essential part of travel planning, with many consumers looking for travel information on a daily basis thinkwithgoogle.com.
•	Travel and tourism is a significant driver of economic growth and employment in the United States, accounting for nearly 3% of the U.S. GDP commerce.gov. The industry supported 9.9 million American jobs that paid more than $322 billion in employee compensation.
•	As travelers become more mobile, the demand for easy-to-use and fast websites and mobile apps has increased thinkwithgoogle.com. Travelers expect a fast, frictionless, and helpful experience when researching and booking their trips, with 53% of mobile visits being abandoned if a site takes longer than three seconds to load thinkwithgoogle.com.
•	Organizing trip can be both thrilling and challenging. Finding amazing destinations and attractions to visit is an exciting activity, but managing all the related information for your journey can be quite demanding.
•	There are various travel planning apps available that cater to different needs, such as Culture Trip for finding off-the-beaten-path destinations . These apps help travelers streamline their trip planning process and ensure a smooth and enjoyable experience.

### Functional Requirements
•	Scalable design to support 2 million active users with 15 Min registered. 
•	Provide best in class user experience both on web and Mobile. 
•	Solution must support easy integration with new partners to get required data for the trip updates.
•	Gather analytical data form all possible avenue’s, to support various reporting requirements. 
•	Provide User with summary based in multiple metrics like (trip , location , travel agency , mode of transport)
•	Support for sharing trip info over social media.
•	Support for multiple languages and time zones to be able to support international usage.

### Non-Functional Requirements
•	Users must be able to access the system at all times (max 5 minutes per month of unplanned downtime)
•	Travel updates must be presented in the app within 5 minutes of generation by the source
•	Response time from web (800ms) and mobile (First-contentful paint of under 1.4 sec) 

### Assumptions
+ System will not perform any direct actions on reservation, platform only allows you to track reservations done in one place.
+ Availability of API interface from existing travel agencies and partners
+ A couple of Functionalities will be primarily driven by client side (web browser/mobile app)
  1. Redirection to preferred partner for Support based on details from backend.
  2. Sharing trip on social media to be managed by direct integrations on client side


## Architectural Analysis

Basis above understanding of problem and our analysis of domain and technical characteristics of the solution, below are the driving factors that will impact our Architecture design.

Agility: We needs to be agile to respond to the changing needs and preferences of our target market, which is travelers who want to manage their travel plans online or through their mobile device. we should use agile methodologies and practices, such as Scrum, Kanban, or DevOps, to deliver value to our customers faster and more frequently. We should also use feedback loops and user testing to validate our assumptions and improve our product.

Abstraction: We need to use abstraction to hide the complexity and implementation details of our system from our users and developers. We should use high-level languages and frameworks that allow us to focus on the business logic and user interface of our system, rather than the low-level details of how it works. We should also use design patterns and principles, such as MVC, SOLID, or DRY, to organize our code and make it more readable and maintainable.

Cost: We need to consider the cost of developing as it’s a startup, maintaining, and operating as it affects profitability and sustainability. We should use microservices, containers and also use open source tools and libraries that reduce the licensing and development costs of our system

Deployability: We need to ensure the deployability of our system on different platforms and environments, as it affects our reach and availability. We should use cross-platform technologies and frameworks that allow us to build our system once and deploy it anywhere

Elasticity: We need to have elasticity in our system to cope with the fluctuations in demand or workload from our users. We should use microservices architectures that allow we to scale up or down our system dynamically based on the traffic or load conditions, such as auto-scaling groups, load balancers, or distributed databases. We should also use monitoring and alerting tools that measure and notify we of the performance and health of our system

Evolvability: We need to ensure the evolvability of our system over time by adding new features, functionalities, or components without affecting the existing ones. We should use modular and loosely coupled design principles that enable easy integration of new components and services into our system, such as new microservices, integration with new partners. 

Fault-tolerance: We need to have fault-tolerance in our system to continue functioning correctly in the presence of errors, failures to maintain uptimes committed to customers We should also use backup and recovery strategies and tools that restore or recover the data or state of our system in case of failures, such as snapshots, replication, or backup services.

Integration: We need to integrate our system with other systems or components that provide essential functionalities or services for our product, such as the agency’s existing airline, hotel, and car rental interface system, or standard social media sites. We should use standard protocols and interfaces that enable interoperability and communication with external systems or components.

Performance: We need to have high performance in our system to meet the expectations of our users who want to see their travel plans updated in real-time and share them with others. We should use asynchronous and event-driven programming models that enable concurrent and parallel processing of tasks, such as promises, and callbacks, We should also use caching and content delivery networks that improve the user experience and reduce the load on the backend servers.

Scalability: We need to have scalability in our system to handle increasing or decreasing amounts of data, users, or transactions without degrading its performance or functionality. We should use microservices and event-driven architectures that allow for horizontal scaling.

Workflow: We need a well-defined workflow that describes the sequence of steps or tasks that define how our system operates. We should use business process modelling techniques and tools that capture the workflow of our system in a graphical or textual form.

The figure shows how our top 4 architecture characteristics score against formal system architecture styles.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/36072f6c-d8a9-4f88-ae34-88691711a631)


+ We chose to go ahead with **Microservices** and **Event-driven architecture** style
+ Although Microservices is low on performance, other characteristics like agility, domain part, fault tolerance and scalability will help small technical teams to quickly add, enhance and test product features while ensuring that system availability is very high.
+ The Shortcomings in performance will be overcome by coupling it with event queues adopting an event-driven micro-service style.


## High-Level Architecture

The Below Figure illustrates our High-Level System Architecture.

![img_final drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/3f885386-d5a8-4dea-9805-99a0f036172c)

**Architecture Strategy**
1. Different Components in the architecture are grouped together logically to represent the concept of *Zoning (Separation of Concern)* i.e., creating separate zones to host databases, microservices that interact with users, Event Queues, microservices that interact with Queues, and External Integrations.
2. The Idea of Zoning will help isolate and protect internal components and data ensuring security by design.
3. *Client-facing microservices* will work in a Synchronous fashion catering to all requests from clients (Mobile App, Web) with inbuilt Authentication.
4. Shared Trip-related microservices will be open to allow users to access them without Authentication.
5. All of the internal processing i.e., polling email server, polling agency for updates, communication, capturing user analytics etc. will be driven by *event-driven microservices*.
6. *Batch Jobs* will process time-insensitive and data-heavy scheduled tasks.
7. Use of *Purpose-built Database stacks* for transactional, analytical, document storage and other purposes.
8. Use of *Data Partitioning techniques* (City-wise partitions) ensures high-performance throughput.
9. *Backend for Frontend Gateway* used to leverage unique features of the user interface.
10. Deployment of Microservices using *Kubernetes* to achieve scalability and fault-tolerance. 

## Detailed Architecture

This Section will focus on elaborating each block in the high-level architecture giving a good understanding of the components and how each component interacts with each other.

We will be using the following legend in the below explanations unless specifically mentioned.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/c7a01629-8d56-434e-aaaa-274e08bcc1e2)

### User-Related Microservices

![User drawio (1)](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/802fd1bc-0d44-47ba-b519-4711c6b20d43)


+ User Profile Data will hold all details about the user like name, email, mobile number etc.
+ User Interface Profile will hold specifics about the interface (web/Mobile App) which will help later personalise content.
+ Email Profile will be optional execution depending on permissions from the user to poll inbox.
+ Locale Profile will hold details about timezone, language preference and other compliances (GDPR) etc. to support international operations.

---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/dec5f174-6dac-4097-8e43-079d479bb93d)

---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/60669337-9448-450e-8cb0-8b3c2062d95a)

---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/c55c26fc-a91c-465f-be62-1bab27eda8d1)

---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/7996a4f9-120a-4cff-9c02-148d73498c82)

---------------------------------------------------------------------------------------------------------------------------------------------
### Trip-Related Microservices

![Trips drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/9c1c3360-1438-4de6-a330-51b35255635a)


---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/157fbc8b-4dc7-489c-baca-0b93680e04c9)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/1f6b4ef2-9487-4b26-8f5f-dec71fc6c980)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/0db400d6-54e5-483e-aa31-e020b4b83d07)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/0f09b81f-7055-4cc1-870c-e8c5b9e47d5f)

---------------------------------------------------------------------------------------------------------------------------------------------

### Reservation-Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/ea91cd1c-ae03-42cf-b941-ef5b72cec64e)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/104b8176-66fe-4f8e-a7c2-061a2710420f)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/2f661bf0-1b14-47e2-99fa-90c0e842f54d)

---------------------------------------------------------------------------------------------------------------------------------------------

### Share Trip-Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/b4fe7421-d70e-49d0-896b-ce7650dfc6ec)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/81df2b49-714b-4cee-8d67-658032a74cfa)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/b6c0e906-84ae-469c-805b-d576e32c518b)

---------------------------------------------------------------------------------------------------------------------------------------------

### Email Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/bf3b1f82-9419-4a4e-becd-37cbf85cb0b0)

---------------------------------------------------------------------------------------------------------------------------------------------
![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/d7d2a1a5-2dd6-421d-bb9a-32930edbdc35)

---------------------------------------------------------------------------------------------------------------------------------------------

### Agency-Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/89dc1c86-18a9-4444-9f9b-f4b376b66287)

---------------------------------------------------------------------------------------------------------------------------------------------

### Communication-Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/741d1c69-6583-4447-b737-cd035d01b34b)

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/4b55a44d-d14b-46a8-a314-361e1716ffdf)

---------------------------------------------------------------------------------------------------------------------------------------------

### Analytics-Related Microservices

---------------------------------------------------------------------------------------------------------------------------------------------

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/ad50d24f-cd5a-4816-be6f-bae815771e4a)

---------------------------------------------------------------------------------------------------------------------------------------------

### Batch Jobs
