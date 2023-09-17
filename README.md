# O'Reilly Architectural Katas 2023

**Contents**
- [Who we are](#who-we-are)
- [Problem Analysis](#problem-analysis)
  - [Business Case](#business-case)
  - [Domain Analysis](#domain-analysis)
  - [Explicit Non-Functional Requirements](#explicit-non-functional-requirements)
  - [Implicit Non-Functional Requirements](#implicit-non-functional-requirements)
  - [Assumptions](#assumptions)
- [Architectural Analysis](#architectural-analysis)
- [High-Level Architecture](#high-level-architecture)
- [Detailed Design](#detailed-design)
  - [State Machine](#state-machine)
  - [User-Related Microservices](#user-related-microservices)
  - [Trip-Related Microservices](#trip-related-microservices)
  - [Reservation-Related Microservices](#reservation-related-microservices)
  - [Share Trip-Related Microservices](#share-trip-related-microservices)
  - [Email Related Microservices](#email-related-microservices)
  - [Agency-Related Microservices](#agency-related-microservices)
  - [Communication-Related Microservices](#communication-related-microservices)
  - [Analytics-Related Microservices](#analytics-related-microservices)
  - [Batch Jobs](#batch-jobs)
- [Strategy](#strategy)
- [Architectural Decision Records](#architectural-decision-records)
  - [ADR 01: Backend for Frontend](#adr-01-backend-for-frontend)
  - [ADR 02: Synchronous Microservices](#adr-02-synchronous-microservices)
  - [ADR 03: Event-Driven Microservices](#adr-03-event-driven-microservices)
  - [ADR 04: Configuration Database](#adr-04-configuration-database)
  - [ADR 05: Caching to Improve Responsiveness](#adr-05-caching-to-improve-responsiveness)
  - [ADR 06: Batch Processing](#adr-06-batch-processing)

## Who we are

We are a bunch of enthusiastic developers, designers and product leads with the goal to make technology valuable and sustainable. We work at Atria Convergence Technologies Limited in India and Our team consists of Akash Kumar R, Manjuprasad Shetty Nidlady, Madhusudhan K, Veni Kota and Koushik Ithal.

## Problem Analysis

### Business Case
Road Warrior is a Startup that wants to revolutionize the travel industry by allowing users to organize, track and seamlessly work across travel partners for a trip. The Product description is as follows:  
+ Building web and mobile app interface that allows travellers to add, update, or delete existing reservations manually as well as see all of their existing reservations organized by trip.
+ Let users forward an email to us or Poll user inbox looking for travel-related emails. 
+ Interfaces with the agency’s existing airline, hotel, and car rental interface system to receive travel update details such as delays, cancellations, updates, gate changes, etc. and preferred travel partner for Support. 
+ Users can also share trip information by interfacing with standard social media sites or allowing targeted people to view your trip. 
+ Provides end-of-year summary reports for users with a wide range of metrics about their travel usage.
+ Gathers analytical data from trips for various purposes, such as travel trends, locations, airline and hotel vendor preferences, cancellation and update frequency, and so on.
+ Works internationally, and supports multiple languages and currencies. 

### Domain Analysis

+ When planning a trip, travellers increasingly rely on their mobile devices. According to Travelport Digital, 80% of travellers used a mobile app to research trips in 2018 (statista.com). Mobile usage has become an essential part of travel planning, with many consumers looking for travel information daily thinkwithgoogle.com.
+ Travel and tourism is a significant driver of economic growth and employment in the United States, accounting for nearly 3% of the U.S. GDP commerce.gov. The industry supported 9.9 million American jobs that paid more than $322 billion in employee compensation.
+ As travellers become more mobile, the demand for easy-to-use and fast websites and mobile apps has increased (thinkwithgoole.com). Travellers expect a fast, frictionless, and helpful experience when researching and booking their trips, with 53% of mobile visits being abandoned if a site takes longer than three seconds to load thinkwithgoogle.com.
+ Organizing a trip can be both thrilling and challenging. Finding amazing destinations and attractions to visit is an exciting activity, but managing all the related information for your journey can be quite demanding.
+ There are various travel planning apps available that cater to different needs, such as Culture Trip for finding off-the-beaten-path destinations. These apps help travellers streamline their trip planning process and ensure a smooth and enjoyable experience.

### Explicit Non-Functional Requirements
+ Scalable design to support 2 million active users with 15 million registered users. 
+ Provide best-in-class user experience both on the web and Mobile. 
+ The solution must support easy integration with new partners to get the required data for the trip updates.
+ Users must be able to access the system at all times (max 5 minutes per month of unplanned downtime)
+ Travel updates must be presented in the app within 5 minutes of generation by the source
+ Response time from web (800ms) and mobile (First-contentful paint of under 1.4 sec) 

### Implicit Non-Functional Requirements
+ Security of the Product to be top-notch since user personal data will be stored.
+ Compliance with local laws for smooth international operations.

### Assumptions
+ System will not perform any direct actions on reservation, platform only allows you to track reservations done in one place.
+ Availability of API interface from existing travel agencies and partners
+ A couple of Functionalities will be primarily driven by client side (web browser/mobile app)
  1. Redirection to preferred partner for Support based on details from the backend.
  2. Sharing trips on social media by direct integrations on the client side

## Architectural Analysis
Based on our understanding of the problem and analysis of the domain and technical characteristics of the solution, below are the driving factors that will impact our Architecture design.

1. **Performance**: We need to have high performance in our system to meet the expectations of our users who want to see their travel plans updated and notified about changes in near real-time along with highly responsive user interfaces. 

2. **Fault-tolerance**: Considering the travel industry an how important it is for users to not miss any updates about their trips and reservations. The system must be fault-tolerant with as minimal downtime as possible.

3. **Scalability**: Given the volume of active users and other users in the database who can turn active at any point of time, its important for system to be able to gradually auto-scale up and scale down depending on the volume of transactions. 

4. **Cost**: Since it's a startup we need to consider the cost of product development, maintenance, and operation as it affects the profitability and sustainability of the business.

5. **Security**: Considering the international operations, compliance with local laws and storage of user Personal data, the security of the product is an implicit and important characteristic. 

6. **Agility**: The Product needs to be agile to respond to the changing needs and preferences of the target market. Considering international operations and few industries related to travel are highly regulated by the government, being agile will offer a competitive advantage in the market.

7. **Interoperability**: The Product has to be able to interface and integrate with various existing aggregators and partners making this a very important characteristic. 

The below figure shows how our top 4 architectural characteristics score against formal system architecture styles.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/36072f6c-d8a9-4f88-ae34-88691711a631)

+ We chose to go ahead with **Microservices** and **Event-driven architecture** style
+ Although Microservices is low on performance, other characteristics like agility, domain part, fault tolerance and scalability will help small technical teams to quickly add, enhance and test product features while ensuring that system availability is very high.
+ The Shortcomings in performance will be overcome by coupling it with event queues adopting an event-driven micro-service style.


## High-Level Architecture

The Below Figure illustrates our High-Level System Architecture.

![Architecture_final drawio(1)](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/732b0e7e-3a59-4de1-9728-907fcd7602b8)

**Architecture Style**
1. Different Components in the architecture are grouped logically to represent the concept of *Zoning (Separation of Concern)* i.e., creating separate zones to host databases, microservices that interact with users, Event Queues, microservices that interact with Queues, and External Integrations.
2. The Idea of Zoning will help isolate and protect internal components and data ensuring security by design.
3. *Client-facing microservices* will work in a Synchronous fashion catering to all requests from clients (Mobile App, Web) with inbuilt Authentication.
4. Shared Trip-related microservices will be open to allow users to access them without Authentication.
5. All of the internal processing i.e., polling email server, polling agency for updates, communication, capturing user analytics etc. will be driven by *event-driven microservices*.
6. *Batch Jobs* will process time-insensitive and data-heavy scheduled tasks.
7. Use of *Purpose-built Database stacks* for transactional, analytical, document storage and other purposes.
8. Use of *Data Partitioning techniques* (City-wise partitions) ensures high-performance throughput.
9. *Backend for Frontend Gateway* used to leverage unique features of the user interface.
10. Deployment of Microservices using *Kubernetes* to achieve scalability and fault-tolerance. 

## Detailed Design

This Section will focus on elaborating each block in the high-level architecture giving a good understanding of the components and how each component interacts with each other.

### State Machine

Trips and Reservations will follow the below state machine, where they will be in *Active* Status on Creation and Move to *Completed* Status on Completion based on events and everyday batch processing of reservations/trips ending that day. Upon User deletion, the status will change to *Deleted*.
This will help us ensure that we don't lose out on data and build rich analytics in the long run.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/a5bba11b-c39f-464c-815f-f489a7a5365b)


We will be using the following legend in the below explanations unless specifically mentioned.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/f2cc50e6-c48d-480e-967f-7b84045f9716)

---------------------------------------------------------------------------------------------------------------------------------------------
### User-Related Microservices

![User drawio (1)](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/802fd1bc-0d44-47ba-b519-4711c6b20d43)

+ User Profile Data will hold all details about the user like name, email, mobile number etc.
+ User Interface Profile will hold specifics about the interface (web/Mobile App) which will help later personalise content.
+ Email Profile will be optional execution depending on permissions from the user to poll inbox.
+ Locale Profile will hold details about timezone, language preference and other compliances (GDPR) etc. to support international operations.
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/dec5f174-6dac-4097-8e43-079d479bb93d)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/60669337-9448-450e-8cb0-8b3c2062d95a)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/c55c26fc-a91c-465f-be62-1bab27eda8d1)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/7996a4f9-120a-4cff-9c02-148d73498c82)

---------------------------------------------------------------------------------------------------------------------------------------------

### Trip-Related Microservices

![Trips drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/9c1c3360-1438-4de6-a330-51b35255635a)

+ Trip Data will hold all the attributes related to Trip and optionally the preferred partner for Support if mentioned.
+ Reservations Data will hold all the attributes related to a Reservation.
+ Recommendations Data will hold all personalised recommendations (based on Trip and based on User Interface)
+ Services will also generate appropriate events to track trips for personalised recommendations.
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams below explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/157fbc8b-4dc7-489c-baca-0b93680e04c9)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/1f6b4ef2-9487-4b26-8f5f-dec71fc6c980)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/0db400d6-54e5-483e-aa31-e020b4b83d07)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/0f09b81f-7055-4cc1-870c-e8c5b9e47d5f)

---------------------------------------------------------------------------------------------------------------------------------------------

### Reservation-Related Microservices

![Reservations drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/d65442ce-5973-4f1b-a32a-8f05943fe461)

+ Reservations Data will hold all the attributes related to a Reservation.
+ Services will also generate appropriate events to track reservations for updates (cancellations/delays etc.).
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/ea91cd1c-ae03-42cf-b941-ef5b72cec64e)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/104b8176-66fe-4f8e-a7c2-061a2710420f)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/86c0adfd-98ba-4d95-84ff-cd091ee437a3)

---------------------------------------------------------------------------------------------------------------------------------------------

### Share Trip-Related Microservices

![Shared Trip drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/2018f98f-c22d-488f-aeec-f4c55ef075bf)

+ We will be generating a unique link to be shared across other platforms.
+ When Opened these links will bypass Authentication, allowing users to easily access it on social media and email.
+ We will also allow users to track the shared trip links and delete them if required.
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/b4fe7421-d70e-49d0-896b-ce7650dfc6ec)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/81df2b49-714b-4cee-8d67-658032a74cfa)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/b6c0e906-84ae-469c-805b-d576e32c518b)

---------------------------------------------------------------------------------------------------------------------------------------------

### Email Related Microservices

![Katas_2023-Email drawio(1)](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/2d8cf02b-3ef2-4e33-93a8-f6183ea4d2ad)

+ Poll Road Warrior Inbox Services will poll the inbox basis filters and credentials stored in the configuration database.
+ Poll User Inbox service will poll the user inbox-based events and accordingly update the reservations if required.
+ If there is any update on the status of the reservation, create appropriate events to notify the user.
+ Emails Data will be a NoSQL DB to store the contents of the email polled.
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/bf3b1f82-9419-4a4e-becd-37cbf85cb0b0)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/874bafb0-2bb3-41ed-a280-1aded0179ae6)

---------------------------------------------------------------------------------------------------------------------------------------------

### Agency-Related Microservices

![Travel_agency drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/b04d102e-c1e6-4c8b-bd24-8056e7f4bc58)

+ Poll Travel Agency Interfaces basis events on the Queue
+ Updates Reservations Data if required and if there is any update on the status of the reservation, create appropriate events to notify the user.
+ Mark Reservations as Completed if they are no longer required to be tracked
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/89dc1c86-18a9-4444-9f9b-f4b376b66287)

---------------------------------------------------------------------------------------------------------------------------------------------

### Communication-Related Microservices

![Communication drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/7e173420-69c0-494a-9ff0-7f9d895ccfaa)

+ On events which require users to be notified, get user interface profile data (to decide on app/web notification).
+ Create Communication events for specific channels and Send Communication to customers on those channels.
+ Configuration database will hold details on the relevant URLs, credentials and interfaces which need to be used for communication.
+ Polling and all additions to Event Queue along with Events to track Analytics are asynchronous actions.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/741d1c69-6583-4447-b737-cd035d01b34b)

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/4b55a44d-d14b-46a8-a314-361e1716ffdf)

---------------------------------------------------------------------------------------------------------------------------------------------

### Analytics-Related Microservices

![Analytics drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/16699423/4fe33065-53e6-4297-b937-5a40cefe6ca3)

+ Analytics DB will hold all details about user actions, changes in reservation details, user preferences, etc.
+ The service will poll the queue and add events along with attributes.

Below flow diagrams explain the working of each microservice.

![image](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/283dca5c-570c-4473-82ca-1834fcb71cd5)

---------------------------------------------------------------------------------------------------------------------------------------------

### Batch Jobs

![Katas_2023-Batch drawio](https://github.com/manjuprasadshettyn/actian_katas_2023/assets/144985834/b6ffa0b5-37da-4945-9028-4d649f266c39)

+ Batch Jobs will perform scheduled and time-insensitive actions regularly like below:
  1. Poll Road Warrior Inbox every 2 mins and filter Travel related emails
  2. Execution of Recommendation Engine every few hours to generate personalized suggestions based on Analytics data
  3. Execution of Recommendation Engine every few hours to generate personalized suggestions based on User Interface Profile
  4. Execution of Travel Summary Engine to generate travel summaries every day and aggregate them for the year.
  5. Trip/Reservation completion based on the end date and removal of appropriate events from queues to optimize performance
  6. ETL Job to import and export data between RDBMS and Data warehouse
     
## Strategy

+ A phased approach is suggested for Polling User email inboxes, since it is a startup it is recommended to build brand trust first and then roll out this functionality.
+ Use of Distributed Deployment approach, having different setups serving similar countries/regions. Controlling deployments and compliance with the local law (GDPR, India Personal Data Protection Act, etc.) through the Configuration Database will help expand in a phased approach with minimal re-work.
+ Effective use of event queue to improve performance:
  + Poll agency/email for updates on only Active Reservations.
  + Poll Road Warrior Inbox every 2 minutes: This will help users quickly add/modify trips by simply forwarding an email.
  + Poll User Inbox every 2 minutes if there are active reservations for a User. If there are no active reservations polling of user inbox can be changed from every 2 mins to 10 mins.
  + Poll agency Interface every 2 minutes if there are active reservations for a User. If no active Reservations then this polling can be paused. 

## Architectural Decision Records

### ADR 01: Backend for Frontend

**Status** : Proposed

**Context**: Our Product has to serve all of the content to the client over services and considering that we have to cater to different types of clients (web and Mobile App), there is a need for a layer which can effectively deliver content to the client taking advantage of the client specific features and at the same time working around the limitations of the client.
Also when integrating multiple services on the client it is always a good practice to isolate the core domain-specific functionality from generic tasks like authorization, routing, security etc.

**Decision**: We will use the Backend For Framework pattern for the incoming requests from App and Web interfaces in Road Warrior

**Consequences**  
Positive:
+ Gives us the flexibility to address tailored needs of web and app clients
+ Gives better routing and security controls
+ Helps Isolate features like share trips for better authentication and authorization

Negative:  
+ Can be an additional hop with nominal addition in latency
+ Overhead of developing and maintaining few additional services

### ADR 02: Synchronous Microservices

**Status**: Proposed

**Context**: Microservices architecture is a design approach for building applications as a collection of small, loosely coupled, and independently deployable services. 
Services hosting web and mobile experiences are by and large powered by request-response designs and Integrations with external third-party solutions almost always use a synchronous mechanism and 
generally provide a flexible, language-agnostic communication mechanism over HTTP.
The use of Synchronous microservices in the Road Warrior system for Client and external integrations will help design the code in a modular way keeping it agile and at the same time easy to scale with high availability. 

**Decision**: We will use the Synchronous Microservices architecture for services in Road Warrior

**Consequences**  
Positive:
+ Gives us the flexibility to scale each of the services
+ Gives us a roadmap to evolve of the system
+ Clear segregation of the domain and its controls

Negative:  
+ It requires careful design, effective communication between services, and proper management of distributed systems
+ Code Duplication is a potential possibility


### ADR 03: Event-Driven Microservices

**Status**: Proposed

**Context**: Event-driven architecture is a design pattern that focuses on the production, detection, consumption, and reaction to events that occur within a system or across multiple systems.
This design pattern can help make decentralized and autonomic decisions and will be very useful for faster processing and updates.
Event-driven microservices applications are tasked with fulfilling the requirements and emitting any of their own necessary events to other downstream consumers. This helps build a flexible and scalable system where business logic is 
defined as granular, loosely coupled and highly testable services.

Road Warrior has various use cases like tracking events for analytics, polling email/interfaces for updates etc. which can be event-driven allowing us to process millions of these tasks in a performance-efficient way.

**Decision**: We will use the Event-Driven Microservices Architecture for event-based actions in Road Warrior.

**Consequences**  
Positive:
+ Asynchronous  capability with EDA can help process things faster without resource-locking
+ Components can process events independently and in parallel, which allows for horizontal scaling by adding more instances of event processors
+ Additional integration is much easier with EDA

Negative:  
+ Complexities related to event ordering, event schema evolution, eventual consistency, and managing the event flow across multiple services which can be avoided by using evolved queuing engines like kafka.



### ADR 04: Configuration Database

**Status**: Proposed

**Context**: Isolation of configuration-related data and data that can help control the business/domain logic to some extent to a central place like a Database can help in the maintainability, deployability and agility of the system in the long run.
These details otherwise generally available in service layer require frequent development efforts/deployment efforts to accommodate changes.
Considering the Travel Industry and how heavily its components are regulated by the government, it will be a good practice to build a system which can quickly respond to changes and work internationally.
In the Road Warrior System, the isolation of Interface integration details, Email configurations and filters, and International compliance standards from the code to a database is a differentiating factor in our design approach.
The International Standards compliance will be controlled through a database ensuring that it's easy to deploy and operate in different countries and the logic required for that country will be applied from the database.
This will also bring flexibility in maintenance without any development efforts on services.

**Decision**: We will use the Configuration Database for Interface, Email configs, and International compliance controls in Road Warrior.

**Consequences**  
Positive:
+ Easy management of integration interfaces. Any modifications can be easily achieved
+ Compliance with international standards can be easily achieved by modification of configurations alone

Negative:  
+ Impact on performance as few services will have additional checks with configuration database

### ADR 05: Caching to Improve Responsiveness

**Status**: Proposed

**Context**: The use of caching technology in DBMS is another factor that we are considering for design. This component is considered as the performance of the application has to be optimal with over 2 million active users.
Caching can help improve the response rate and avoid significant load on the Database keeping its resource utilizations in control

**Decision**: We will be use caching techniques of the databases in Road Warrior

**Consequences**  
Positive:
+ Controlled System resource utilization
+ Improved Response rate of database operations

Negative:  
+ Higher cost of caching from initial resource provisioning
+ Careful tuning of caching would be required for optimal utilization


### ADR 06: Batch Processing

**Status**: Proposed

**Context**: For heavy compute analytics tasks and non-time-sensitive updates on the system, batch jobs are an ideal choice. It can run in isolation from the regular transactions while being completely decoupled from the transaction systems 
and bring in the relevant data/actions whenever required.

**Decision**: We will use a series of batch processing for non-time-sensitive processes and Heavy computing tasks in Road Warrior.

**Consequences**  
Positive:
+ Batch jobs are ideal for processing large volumes of data
+ Can help avoid transaction load on the system by decoupling from regular transactions

Negative:  
+ Updates might not be near real-time
+ Additional controls for fault tolerance needed
