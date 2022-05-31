# Designing Microservices: The SEED(s) Process

The microservices design system described in this chapter is a top-down, multistep 
methodology, and a collection of reusable processes, where each later step evolves
from a previous one. Due to its evolutionary nature, we call the system Seven Essential 
Evolutions of Design for Services or SEED(S).

#### **Key Decision: Use a Standard Process for Service Design:**
*Use a standard, repeatable process to achieve consistently high-quality, customer-centric 
design for the services in your system.*  
</br>

## Introducing the Seven Evolutions of Design for Services:
</br>
<details>
<summary><b>1. Identifying actors</b></summary>
</br>
SEED(S) takes a distinctly customer-centric approach, viewing as products the services 
it is used to design. By now the “APIs are products” mantra is not particularly novel. Typical plagues of API and service design in our industry are overabstraction and lack of clarity regarding user needs. Too many APIs are simply exposures of some database tables over HTTP or an attempt to provide direct networked access into application internals, via remote procedure calls (RPCs). 

#### **Key Decision: Scope Service Design Using Key Actors**
*Start service design by identifying key actors in your domain, to achieve customer-centric 
scoping of the capabilities represented by the services.*   
</br>

There are several fundamental rules for identifying the right set of actors for your goals:
1. Identifying the boundaries of what key traits differentiate various actors of 
our design is more important than identifying an excruciating level of detail for 
who the actors are.  
2. Overlapping or too-broad actor definitions are usually red flags.  
3. These needs and behaviors that distinguish one actor type from another are relevant.  
4. Less is more—you should use as few distinct actors as possible to describe your 
problem area, but no fewer than necessary. 

### **Example Actors in Our Sample Project**  

*Frequent flyer*

	Emma travels for work, has elite loyalty status with the airline, manages her 
	travel through her work’s reservation system, and uses a number of connected 
	apps to stay on top of her busy schedule. Due to her loyalty status, she 
	is eligible for many perks. Often planning trips on short notice, when 
	traveling with family, she typically uses loyalty miles.

*Family vacationer*

    Riley and their spouse are mostly traveling for vacations with their kid(s). 
	They usually plan trips well in advance, and travel infrequently.

*Airline customer service agent*

    Sean is an experienced customer service agent assisting travelers with booking, 
	rebooking, and resolving issues during travel and after through phone and online chat.


</details> </br>
<details>
<summary><b>2. Identifying jobs that actors have to do</b></summary>
</br>  

It is important to understand the jobs that actors have to get done, and only then 
should we create a solution that best addresses their needs. Any effective API or service design methodology operates under the premise that APIs and microservices are types of products, and in their design we can successfully employ the rich *product management* toolset that has been developed over many decades.   

*Consumers of APIs* are typically frontend (web, mobile) or third-party (partner) 
applications, while *consumers of microservices* are various parts of the system itself. When beginning work with microservices, it's helpful to keep in mind that it’s the problems that are timeless; solutions change and evolve all the time.   

</br>  

#### **Key Decision: Use Jobs as the Unit of Analysis**
*Use jobs that key actors have to get done, in your domain, as the unit of analysis for 
collecting requirements.*   
</br>   

User Stories revolve around a user persona; they start with *“as a persona,”* while Job Stories disregard the persona and instead emphasize the circumstance.  The SEED(S) process identifies actors to scope the list of jobs, but at the point of describing each job for that actor, we need to identify circumstances.   
</br>   

### **Using Job Story Format to Capture *Jobs to be Done* (JTBDs)**   

For each of the actors we identify, we need to discover top JTBDs for that actor.  The SEED(S) process uses the Job Stories format as defined by [Paul Adams](https://www.intercom.com/blog-the-dribbblisation-of-design):   

*"when __circumstance__, I want to __motivation__, so I can __goal__"*  
</br> 

#### **Key Decision: Use the Standard Job Story Format**  
*Use a standard format for capturing JTBDs (known as Job Story) to uniformly capture circumstances, motivations, and goals for all your jobs.*   
</br>   

### **Example JTBDs in Our Sample Project** 

*Frequent flyer*                                                                

1. *When* __Emma’s plans change__ and she is unable to travel on a previously 
booked flight, *she wants to* __easily reschedule her flight__, *so she 
can* __get a flight that works for her new plans__.

2. *When __Emma prefers an available seat__* other than the one she has been currently 
assigned, *she wants to __select the alternative seat__*, so *she can __enjoy her flight more__*.
                                                                                
*Family vacationer*                                                             

1. *When* __Riley is planning a flight__* for their family vacation, *they want to* be able to __filter available flights with multiple criteria, including: four adjacent seats available on the flight, the number of connections, connections that go through airports that have facilities friendly to young children, etc.,__ *so that their* __family can fly with maximum comfort__.

2. *When* __Riley is planning a quick, unplanned family getaway for a long weekend__, 
*they want to* __get suggestions for interesting available trips that are affordable and a short flight__ *so they* __can have a list of choices they can consider__.
                                                                                
*Airline customer service agent*  

1. *When* __a customer calls Sean__, *he wants to* __have a servicing ticket open pre-filled with customer information__, *so he can* start tracking the progress towards the resolution of the customer need.

2. *When* __a customer is asking Sean to find them a convenient flight for their trip__, *he wants to* __be able to find a fitting flight using a flexible set of filtering criteria__, *so he can* meet the customer need and book a flight.   
</br>   

Job Stories provide a great format for conversations with subject matter experts and actual customers, but they are not convenient for deriving actual technical requirements. 

</details> </br>
<details>
<summary><b>3. Discovering interaction patterns with sequence diagrams</b></summary>
</br>

To proceed with a good design, we need to understand the service interaction patterns of our subdomain. You will want to draw an interaction diagram, explaining the sequence of events within your model.  SEED(S) recommends employing Unified Modeling Language (UML) sequence diagrams for this task.

We highly recommend using one of the Markdown-based diagramming formats, such as [PlantUML](https://plantuml.com/).

We recommend this kind of approach because modeling in a microservices team is a team activity. Using a text-based format instead of a graphical file will allow team members to:   

* Keep modeling separate from everyone’s personal choice of editor.
* Easily and effectively version-control sources of the diagrams.
* The diagrams become code and anything you can do with the code, you can now do with your diagrams as well;

#### **Key Decision: Use PlantUML Sequence Diagrams to Discover Interaction Patterns**
*To discover interaction patterns in SEED(S) methodology, we choose to use UML sequence diagrams expressed in a textual (Markdown) format such as PlantUML.*

The Job Stories and actors represent the requirements of the physical world. They do not generally map to technical interactions one-to-one. Your interaction model of the events do not necessarily have to occur between the actors described in the first step of the SEED(S) process. Neither do they have to correspond to the jobs directly. Rather, your interaction diagrams may go a level deeper and show how the user-centric requirements translate into interactions between services at a technical level.

For instance the PlantUML text may read as follows:  
```yaml
@startuml
AgentServicing -> ReservationsApi: checkRes(reservationId)
ReservationsApi -> ReservationCRUD: reserve(data)
ReservationsApi -> ReservationCRUD: cancel(reservationId)
@enduml
```   
</br>   

![PlantUML](sequenceDiagram.png) | 
|:--:| 
| <b>A rendered PlantUML of the sample UML sequence</b>   
</br>  

Once we have the sequence diagrams of the interactions, we can capture the technical requirements for a microservice, or an API, in the form of a set of actions and queries described using a standard syntax.  

</details> </br>
<details>
<summary><b>4. Deriving high-level actions and queries based on jobs to be done (JTBDs) and the interaction patterns</b></summary>
</br>

Once we understand service interaction patterns and have had a chance to visualize those, we can transform jobs into more technically oriented interface contracts and greatly simplify our design process. In SEED(S) we model a system’s interface contracts as collections of two distinct types of interactions: the actions (“commands” in CQS) and the queries.   

#### **Key Decision: Separate Service Endpoints into Commands and Queries**   
*Use the CQS principle to model the action side of the services separate from the query side, and document each with their own standard format.*    

In SEED(S), queries are lookups with defined inputs and outputs. They should be clearly formulated contracts between a client and a server: what input a client sends and what response they expect. They are distinctly different from actions, in that queries do not modify the system state (they “have no side effects”).   

Actions, in contrast, are requests that cause some sort of state modification—they not only do have side effects, but their whole purpose is to cause side effects.   

We recommend using a standard format for capturing queries and actions. The template for queries looks something like this:

* An expressive description of a query
	* Input: list of input variables
	* Response: list of output data elements

Likewise, the standardized format for actions would look like the following:

* An expressive description of an action
	* Input: list of input variables
	* Expected outcome: description of the induced side effect
	* Response (optional): list of data elements in the response (if any)   

Note that Job Stories do not always produce exactly one query or action.    

### **Example Queries and Actions for Our Sample Project**   
</br>   

#### **Queries**  

One of our Job Stories described a *__family vacationer__* actor.  To satisfy the needs of such a job, we need a query contract that allows indication of all such preferences as inputs to the search query. Therefore, our query definition may look like the following:   

*Query 1: Flight Search*

* Input: `departure_date, return_date, origin_airport, destination_airport, number_of_passengers, baby_friendly_connections, adjacent_seats, max_connections, minimum_connection_time, max_connection_time, order_criteria [object], customer_id` (optional; to check loyalty privileges)
* Response: list of flights satisfying the criteria


*Query 2: Lookup of Alternative Flights for a Date Change*

* Input: `reservation_id, new_departure_date, new_return_date`
* Response: list of alternative flights   
</br>   

#### **Actions**  

*Travel Rebooking*

* Input: `original_reservation_id, new_flight_id, seat_ids[]`
* Expected outcome: new flight booked or error returned; if new flight is successfully booked, old one is canceled
* Response: success code or a detailed error object

*Seat Change*
* Input: `reservation_id, customer_id, requested_seat_ids[]`
* Expected outcome: new seat reserved if the seat is available and the traveler is qualified; old seat canceled if the new seat ends up being successfully reserved
* Response: success code, or a detailed error object

In some sophisticated cases, you may find that the actions and queries approach of defining interface contracts may not be sufficient. In these cases, to capture the more complex requirements, we highly recommend using [Matt McLarty’s Microservice Design Canvas](https://dzone.com/articles/streamlined-microservice-design-in-practice).  

</details> </br>
<details>
<summary><b>5. Describing each query and action as a specification, with an open standard (such as the OpenAPI Specification [OAS] or GraphQL schemas)</b></summary>
</br>

As a general rule, it is important to formally describe the interface contract of an API or a microservice before we start implementing it in code.   Contracts implemented using open standards such as the Open API Spec and GraphQL are widely supported by a rich set of tooling that allows easy rendering of documentation, streamlined creation of developer portals, etc.   

Microservices interconnections do not have to be RESTful APIs. Other popular choices include GraphQL, gRPC, and asynchronous event communications. At the time of writing, using JSON-, ProtoBuf-, or Avro-encoded messages on Kafka Streams seems to be a popular choice.   

Producing a formal API contract is a huge milestone for the design of APIs and microservices. Some may even consider it a job well done at this point. However, good API designs cannot end at this stage.   

</details> </br>
<details>
<summary><b>6. Getting feedback on the specification</b> </summary>
</br>

We need to show the draft design of the endpoints to the client developers who will be asked to use these APIs and services, and collect their feedback.   

#### **Key Decision: Collect Feedback on Your Service Designs**  
*Service design is not done until it is presented to the target audience for the service and feedback is collected and applied to the initial designs.*     

You need to keep in mind two groups of customers when designing services and APIs:
* End users of the system. Your APIs enable the user experiences for them.
* Client developers who will code against your services (APIs or microservices). They build end users’ experiences, such as web or mobile applications.   

At the beginning of the SEED(S) process, we interview the end users to collect and understand the Job Stories relevant to them. However, later in the process we start receiving feedback from the client developers. This can happen as early as the interactions design phase, and then again once the OAS is produced, before coding. This second group, API client developers, must be interviewed to test the usability of the designs, to avoid coding something that may end up being rejected by them due to poor usability.    

</details> </br>
<details>
<summary><b>7. Implementing microservices</b></summary>
</br>

### **Microservices Are Not Just Smaller APIs**   
*Microservices are not just smaller replacements for the APIs of the old days. Microservices provide the implementation of your system, while APIs should still be the outward-facing interface of a system.*   
</br>   

We think that if microservices replace anything, the things they replace are the modular components you used to build your systems with. If before you would build a large system by linking (statically or dynamically) various submodules together, in a microservices architecture the building blocks are networked services we call “microservices.”    

Note that a similar approach—of separating APIs into “internal ones, the ones you build with”; and “external ones, the ones that are optimized for consumption by frontends”—has been described by Phil Calçado as the [Backend for Frontend pattern](https://philcalcado.com/2015/09/18/the_back_end_for_front_end_pattern_bff.html) when he was at SoundCloud, and by [Daniel Jacobson](https://www.infoq.com/news/2015/11/daniel-jacobson-ephemeral-apis/) during his time at Netflix. Daniel Jacobson explained how at Netflix they separated APIs into Experience (frontend) and Ephemeral (backend) APIs.    

#### **Key Decision: Web APIs Are Layered on Top of Microservices**  
*Differentiate between web APIs that represent the public interface of your subsystem and microservices that represent the implementation of the same system. Avoid thinking of microservices as “just small APIs.”*   

In our experience, the ideal separation of duties happens when all of the business logic (capabilities) is implemented by microservices, while APIs act as a thin layer of orchestration in front of those microservices. Additionally, we recommend that teams try to avoid microservices directly “invoking” each other. Instead, for the sake of loose coupling, it’s best if any orchestrating workflow is implemented in the API layer, in front of microservices, without microservices knowing anything about each other.   

#### **NOTE** 
*Note that there is no 1:1 relationship between an API and the microservices that implement the corresponding capability. These two assets are parts of fundamentally different layers of your architecture.*     

We believe that such a “microservices should be unaware of each other and be orchestrated externally” approach is where the Unix philosophy of building a system as a collection of composable tools resonates well with microservices architecture principles. One of the most powerful aspects of the Unix philosophy is that you can combine Unix tools (e.g., GNU tools) in a variety of ways using input and output piping on the command line or in shell scripts. However, in order to achieve this, it’s critical that various Unix tools act the same way for any input—they should not care who “calls” them or where their output goes. Components cannot explicitly know about each other for them to become composable. Loose coupling is what makes the whole thing work, not just that the tools are small-ish and focused. The same holds true for microservices.   

#### **Keep Microservices Unaware of Each Other**   

Avoid microservices directly “knowing” about each other and directly calling each other via synchronous interfaces. Instead, try to orchestrate processes involving multiple microservices in the API layer. If this is not possible, consider using asynchronous interfaces between microservices where an upstream microservice publishes data to an event log (e.g., Kafka) and a downstream microservice can subscribe to that event log without the upstream microservice having tight coupling with the subscriber(s).




</details> </br>


