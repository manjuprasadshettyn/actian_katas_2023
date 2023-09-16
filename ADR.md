# ADR 1: Backend for frontend

## Status  
Proposed

## Context 
The Backend for Frontend (BFF) pattern is a design approach that involves creating a separate backend specifically tailored to the needs of a frontend application.


It is needed to address some of the challenges and complexities that arise in modern web and mobile application development.


Backend for Frontend pattern helps to address the complexities and specific requirements of frontend applications, providing a dedicated backend layer that enhances performance, security, and developer productivity.

## Decision   
We will use BFF for the incoming request from App and Web interfaces in Road Warrior

## Consequences  
Postitive:
+ Gives us flexibilty to address tailored needs of web and app clients
+ Gives better routing and security controls
+ Helps Isolate features like share trips for better authentication and authorization

Negative:  
+ Can be and additional hop with nominal addition in latency



# ADR 2: Microservices Architecture

## Status  
Proposed

## Context 
Microservices architecture is a design approach for building applications as a collection of small, loosely coupled, and independently deployable services.


With various services being used in the applications this design approach help easily scale the required services.


Since the application can be broken down into individual automic units building services over a microservice architecture will ease a lot of challenges for Road Warrior.


As Road warrior has very minimal tollrence to faults and downtimes

## Decision   
We will use the Microservices architecture for services in Road Warrior

## Consequences  
Postitive:
+ Gives us flexibilty to scale each of the service
+ Independent development of services can be done 
+ Clear segregations of domain and its controls
+ Addition of a service to a domain will be easy

Negative:  
+ It requires careful design, effective communication between services, and proper management of distributed systems
+ Code Duplicatation is a potential possibility



# ADR 3: Event Driven Architecture (EDA)

## Status  
Proposed

## Context 
Event-Driven Architecture is a design pattern that focuses on the production, detection, consumption, and reaction to events that occur within a system or across multiple systems.


Road Warrior has various use cases which are event driven in nature. These event driven actions can we well managed with the state machine for eventual inconsistency.


This deseign pattern can help take decnetralized and autonomic decisions. 


Async capabilities with EDA is going to very useful for faster processing and updates.

## Decision   
We will use the Event-Driven Architecture for event based actions in Road Warrior

## Consequences  
Postitive:
+ Asynchronous  capability with EDA can help process things faster without resource locking
+ Components can process events independently and in parallel, which allows for horizontal scaling by adding more instances of event processors
+ Additional intergaration is much easier with EDA
+ Integrating disparate systems and services is another value add of EDA

Negative:  
+ Complexities related to event ordering, event schema evolution, eventual consistency, and managing the event flow across multiple services



# ADR 4: Usage of Config Database

## Status  
Proposed

## Context 
Isolation of Interface integration details, Email configurations and permissions , International complience standards from the code to a database is a differentiating factor in our design approach.


This will help bring flexibility in management without any alternations to service.


As part of the architecture to make it International Standards complient we want to keep Road Warrior solution flexible to various configurational aspects.


Configurational attribute being in Config database will simplify these controls and process

## Decision   
We will use Config Database for Interface, Email configs , Internation compleience standards in Road Warrior

## Consequences  
Postitive:
+ Easy management integration interfaces. Any modifications can be eaily achived
+ Complience to intenational standards can be easily achived by modification of configurations alone

Negative:  
+ Hit on performance as few services will have additional with config database



# ADR 4: Usage of Config Database

## Status  
Proposed

## Context 
Isolation of Interface integration details, Email configurations and permissions , International complience standards from the code to a database is a differentiating factor in our design approach.


This will help bring flexibility in management without any alternations to service.


As part of the architecture to make it International Standards complient we want to keep Road Warrior solution flexible to various configurational aspects.


Configurational attribute being in Config database will simplify these controls and process

## Decision   
We will use Config Database for Interface, Email configs , Internation compleience standards in Road Warrior

## Consequences  
Postitive:
+ Easy management integration interfaces. Any modifications can be eaily achived
+ Complience to intenational standards can be easily achived by modification of configurations alone

Negative:  
+ Hit on performance as few services will have additional with config database




# ADR 5: Caching Technology in DBMS

## Status  
Proposed

## Context 
Use of caching technology in DBMS is another factor that we are considering for design.


This component is considered as the performnace of the application has to be optimal with over 2M active users.


Caching can help improve the response rate and avoid significant load on Database keeping its resources utlizations in control

## Decision   
We will use caching techniques for databases in Road Warrior

## Consequences  
Postitive:
+ Controlled System resource utilizations
+ Improved Response rate of database operations

Negative:  
+ Higher cost of caching from intial resource provisioning
+ careful tuning of caching would be required for optimal utilization


# ADR 6: Batch Processing for non time sensitive process and Heavy compute tasks

## Status  
Proposed

## Context 
For the heavy compute analytics tasks or non time senstives updates on the system batch jobs are an ideal choice. It can run in isolation to the regular transactions.


It can also be completely decoupled from the transactions systems and bring in the relevant data whenever required.

## Decision   
We will using a series of batch processing for  non time sensitive process and Heavy compute tasks  in Road Warrior

## Consequences  
Postitive:
+ Batch jobs is ideal for processing large volumes of data
+ Can help avoid transaction load on system by decoupling from regular transactions

Negative:  
+ Updates might not be near real time
+ Additional controls for fault tolerence needed
