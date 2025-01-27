# System Design and Software Architecture

## Software Architecture
    
### Definition and motivation
    ```
        - high level description of a software system, it's components and how these components communicate with each other to fulfill the system's requirements and constraints.
        - requirements : what system must do
        - constraints : what sytem must not do
        - why : 
          - more scalable system, easily changable, extendable,
          - more fault tolerant system
    ```

### System Design and Architectural drivers
#### Requirements
    ```
       - requirements : formal description of what we need to build
          - Challenges of Gathering requirements
            - Large scale system requirements are different than the 
              usual req we typically get for implementing 
              - a method
              - an algo
              - class
            - Big scope and High Level of abstraction
                - Large System -> Application -> Library -> Module -> class -> method/function
            - There is high level of ambiguity in system Design
                - requirements are provided by someone not very technical
                - so it is our job to transform those to technical requirements
                - getting the requirements is already a part of the 
                - client doesn't always know what they need
                - they only know what problem they need to solve
            - Example : Hitchhiking service
                - High level requirement : "Allow people to join drivers on a route, who are willing to take passengers for a fee"
                - clarifying questions:
                    - Real time vs Advance Reservation
                    - User Experience : Mobile? Desktop ? Both?
                    - Payment through us or direct payment to driver

        - Importance of Gathering requirements
            - Gathering requirements is already part of the final solution
            - large scale systems cannot be easily changed
            - many people are involved with many months of engineering work
            - hardware and software costs
            - contracts include financial obligations
            - reputation and brands

        - Types of requirements: Architectural drivers
            1. Features of the system
                - Functional requirements
                - describe system's behavior - what "system must do"
                - easily tied to object of our system
                - user Actions ---> system ---> result/outcome
                  events       --->
                - do not determine its architecture
                - generally any architecture can achieve any feature
                - Examples : Hitchhiking service
                    - when a rider logs into the service mobile app, the sytem
                      must display a map with nearby drivers within 5 miles radius
                      - input : login of user, output : view of map
                    - when a ride is completed, system will charge the rider's ccard
                      and credit the driver, minus service fees"
                      - input : ride completion, output : money transfer
            2. Quality Attributes
                - Non Functional requirements/ Quality attributes
                - system properties that the system must have
                - Examples:
                    - Scalability
                    - Availability
                    - Reliability
                    - Security
                    - Performance
                - These dictate the software architecture of the system
                - different software architectures provides different quality attributes
            3. System constraints
                - limitations and boundaries
                - time constraints 
                    - strict deadlines
                - financial constraints
                    - limited budget
                - staffing constraints
                    - small number of engineers
                         
    ```
#### Feature/Functional requirements Gathering
    ```
        - Naive way 
            - ask the client to describe everything they need
            - not good for complex systems
        - Methods of Gathering requirements
            - Use Cases:
                - Situation/Scenario in which our system is used
            - User Flows:
                - A step by step/ graphical representation of use cases
        - Requirement Gathering steps
            1. Indentify all the actors/users in our system
            2. Capture and describe all possible use-cases/ scenarios
            3. User Flow - Expand each use case through flow of events
                - Each event contains
                    - Action
                    - Data
                - Unified Modeling Language - Sequence Diagram
                    - Diagram that represents interactions between actors and objects
                    - Part of UML - standard for visualizing system design
                    - In practice : mostly used for software design
                    - No real standard Diagrams representing sw arch in the industry
                    - UML is not strictly followed in the industry
                    - Sequence Diagrams are frequently used to represent interactions between entities
                    - Time from top to bottom, entities are vertical lines left to right, interactions between 
                      entities as solid arrows between vertical lines, responses are dotted arrows 

        - Example : Hitchhiking service
            - Actors/users
                - passenger/rider
                - driver
            - Use cases
                - Rider first time registration
                - Driver registration
                - Rider login
                - Driver login
                - Successful match and ride
                - Unsuccessful ride
                ...
            - Expand : Successful match and ride
                - sample_uml.png, sample_uml2.png
            - Actions initiated by the actors are the API's
            - Responses are the result of the processing of the API calls

    ```

#### Non functional requirements/ quality attributes
    ```
        - Motivation:
            - systems are frequently redesigned not because of functional reqs but because:
                - system isn't fast enough(performance)
                - doesn't scale
                - slow to develop
                - hard to maintain(deployability)
                - no secure enough
                - isn't available
        
        - the qualities of the functional requirements of the system
        - The overall properties of the system
        - provide quality measure on how well our system performs on a particular dimension
        - have direct correlation with the architecture of our system 
        - must satisfy quality requirements of all stakeholders
        - have to be measurable
        - have to be testable
        - no single architecture can provide all the quality attributes
          because quality attributes can contradict each other
        - Some combinations of quality attribs are very hard/impossible to acheive
            - Example : performance and security
        - software architecture is about making the right "tradeoff" that give us the highest chance of success according to requirements
        - Feasibility
            - make sure that the system is capable of delivering what client is asking for
            - the client may ask for something that is impossible technically
                prohibitively expensive to implement
                call this out early own
            - example : unrealistically low latency across continents
            - 100% availability
            - full protection against hackers
            - very high storage growth
            - High res video streaming in limited bandwidth

    ```

#### System Constraints in Software Architecture
    ```
        - for quality attribs, we are expected to make tradeoffs
        - A system constraint is essentially a decision that was already either
          fully or partially made for us, restricting our degrees of freedom for
          desingning the system.
        - pillars for our software architecture:
            - they provide us with a solid starting point
            - The rest of the system need to be designed around them.
        
        - types of constraints:
            - technical constraints
                - example: being locked to a particular hardware/cloud vendor
                           Having to use particular programming language
                           Having to use particular DB or tech
                           having to support certain platforms, os etc
                           not having an expert in certain required tech
                - They affect the decisions we make in the design phase and put restrictions
                  on our architecture
                
            - business constraints
                - limited budget or strict deadline
                - usage of third-party services with their own API's and architectural paradigms
                 
            - regulatory/legal constraints
                - global/geographic region based

        - considerations
            - don't take any constraints lightly
            - there should be distinction between real and self-imposed(may be negotiated/removed)
              constraints
            - use loosely coupled architecture : leave enough room for removing present constraints in the future
                ex : if limited to a DB/third-party service, make sure system is not tightly coupled to that tech or API
             - usage of different tech/service in future should need minimal changes

    ```

### Most important Quality Attributes in large scale systems
#### Performance
    ```
        - Performance : Response time(aka end to end latency)
            - time between a client sending a request and receiving a response
            - Response time = Processing time + Waiting time
                - processing time : time spent in our system actively in the 
                  request and building/sending the response
                - Waiting time : duration of time request/response spends inactively 
                  in our system like packet transfer time in network, wait in process queue
                  maybe called latency
            - critical part of user interactions

        - Performance : Throughput
            - amount of work performed by the system per unit of time
                - measured in number of tasks/second
            - amount of data processed by the system per unit of time
                - measured in Bytes/sec or MB/sec
        
        - considerations
            - measuring the response time correctly
            - response time distribution : ideally all the samples of response times should be the same
              analyze the response time samples:
                - sort the samples and bucket them in a histogram(gives frequency of response times)
                - the xth percentile is the value below which x% of the values can be found
                - plot percentile distribution graph and plot average on the graph, the part of graph
                  above the average line is called tail latency(the small percentage of response times
                  from a system that take the longest in comparison to the rest of values)
                - shorter tail latency -> better
                - define response time goals using percentiles
                - measure and compare according to percentile plots
            - performance degradation point : the point of load on the system after which the performance of
              the system becomes significatly worse(low throughput and high response times)
            - if performance degrades very quickly this means high resource utilization:
                - high CPU utilization
                - high memory consumption
                - too many connections/IO
                - message queue is at capacity
                  
    ```

#### Scalability
    ```
        - Traffic patterns:
            - the load/traffic on our system never stays the same and can follow different
              patterns like seasonal, global events etc.
            - in a Successful system, load or users will increase over time
        - The measure of a system's ability to handle a growing amount of work,
          in an easy and cost effective way, by adding resources to the system
        - types
            - Scale UP/ Vertical Scalability
                - upgrading hardware
                - adding resources/upgraing existing resources on a single computer
                  to allow our system to handle higher load
                - Pros :
                    - any application can benefit from it
                    - no code changes are required
                    - migration between machines is very easy
                - cons :
                    - scope of upgrade is limited
                    - we are locked to a centralized system which cannot provide
                        - high availability
                        - fault tolerance

            - Scale out/ Horizontal Scalability
                - add more machines
                - adding more resources in a form of new instances running on different
                  machines, to allow our system to handle higher load
                - Pros:
                    - no limit on Scalability
                    - it's easy to add/remove machines
                    - if designed correctly, we get:
                        - high availability
                        - fault tolerance
                - Const:
                    - initial code changes may be required
                    - increased complexity, coordination overhead

            - Team/ organization Scalability
                - from devs perspective : scalability is measure of system's ability
                  to handle growing amount of work(features,testing,bugs etc), in an easy and cost effective way,
                  by adding resources(engineers) to the system
                - reasons for productivity degradation in monolithic systems:
                    - many crowded meetings
                    - code merge conflicts
                    - business complexity - longer ramp up time
                    - testing is harder and slower(need to test everything)
                    - releases become very risky
                - software arhitecture impacts engineering velocity(team productivity)
                - seperate codebase into seperate services which communicate with each other
                  over the network using loosely coupled protocols. Each service has its own stack
                  and modules
                - breaking down codebase into modular system 
            
            - system can be scaled in all dimensions concurrently
            - three types are orthogonal ways to scale our system

    ```

#### Availability
    ```
        - has greatest impact on our users and business
            - can cause loss of revenue and loss of customers to competitors
        - impact of outage is compounded and results in loss to the company
        - system downtime can potentially put lives on line when system is used
          mission critical service like healthcare/air traffic control
        - the fraction of time/probability that our service is operationally functional
         and accessible to the user
        - uptime : time that our system is operationally functional and available to the user
        - downtime : time that our system is unavailable to the user
        - Availability = uptime / (entire time our system is running)
                       = uptime / (uptime + downtime)
        - MTBF(mean time between failures) : avg time our system is operationally
            - useful when : dealing with multiple pieces of hardware 
                            we are not using cloud/third-party API with a given uptime
                            and availability
        - MTTR(mean time to recovery) : time average it takes us to detect and recover from failure
            - average downtime of our system
        - Availability = MTBF / (MTBF + MTTR)
            - more statistical and talks about probabilities
            - More useful for availability estimation rather than measurement
            - if we can reduce MTTR to 0 or very low then we have 100% availability
            - shows that detectability and fast recovery can help achieve high availability
        - 100 % : extremely hard to acheive, leaves no time for maintenance/upgrades
        - industry standard is 99.9% or higher
            - daily downtime : 1m 26s
            - weekly downtime : 10m 5s
            - Monthly downtime : 43m 12s
            - Annual downtime : 8h 45m 36s
        - 3 nines = 99.9, 4 nines = 99.99 
    ```

#### Fault tolerance and high availability
    ```
        - Common categories of causes of failures
            - Human error
                - pushing a faulty config to production
                - running wrong command/script
            - Software error
                - long garbage collections
                - out-of-memory exceptions
                - null pointer exceptions
                - segmentation faults
            - Hardware failures
                - servers/routers/storage devices breaking down due to limited shelf-life
                - power outages due to natural disasters
                - network failures because of :
                    - infra issues
                    - general congestion
        
        - Failures will happen despite :
            - improvements to our code
            - review, testing and release processes
            - performing ongoing maintenance to our hardware
        - so Fault tolerance is the best way to achieve High availability in our system
        - Fault tolerance enables the system to remain operational and available to the users
          despite failures within one or multiple of its components
          - when failures happen a fault tolerant system will:
            - continue operating at the same/reduces level of performance
            - prevent the system from becoming entirely unavailable

        - Tactics to achieve fault tolerance:
            - Failure prevention
                - eleminate any single point of failure in our system
                - replication and redundancy, multiple instances of servers/services/DB
                - Spatial redundancy : running replicas of our application on different computers
                - Time redundancy : repeating the same operation/request multiple times until we
                    succeed/give up
                - Active - Active Architecture
                     - Example : multiple DB's all synced, if one down other can be used
                     - load is spread among all the replicas
                     pros:
                        - indentical to Horizontal scaling
                        - allows more traffic and better performance
                     cons:
                        - All replicas are taking requests
                        - additional coordination required to keep active replicas in sync

                - Active - Passive Architecture
                     - Example : one primary instance, others take periodic snapshots of primary one
                     - pros:
                        - implementing is easier
                        - there is a clear leader with up-to-date data
                        - rest of the replicas are followers
                     - cons :
                        - ability to scale the system is lost
                        - all the requests still go to only one machine

            - Failure detection and isolation
                - seperate monitoring service requried that keeps checking health of the multiple instances
                    - False positives
                        - the issue might be the network/long garbage collections
                        - may assume a healthy host may be down
                    - monitoring service should not have false negatives
                        - may not detect the crashed servers
                    - exchanges messages with hosts in form of pings and heartbeats
                    - collect data about the number of errors each host gets per minute
                        - if the error rate in one of the hosts is high, it can interpret that
                          as failure of the host 
                    - collect information about time taken for each host to respond 
                        - if the time to respond to requests become too long, it can decide that
                          the host is slow
                        
            - recovery
                - Fast Recovery means high availability
                - after detecting faulty instance/server:
                    - stop sending traffic/workload to that host
                    - restart the host to make the problem go away
                    - rollback : going back to a version that was stable and correct
                        - common in DB's
                        - if we get to a state violating some condition/data, we can
                          roll back to the last correct state in the past
                        - if detect errors rolling out new versions , roll back to previous version

    ```

#### SLA, SLO, SLI
    ```
        - SLA : Service Level Agreement
            - it is a legal contract that represents our quality service such as:
                - Availability
                - performance
                - data durability
                - time to respond to system failures
            - it states the penalties and financial consequences, if we breach the contract
                - penalties include
                    - full/partial refunds
                    - subscription/license extension
                    - service credits
            - SLAs exist for:
                - External paying users(always)
                - Free external users(sometimes)
                    if system has major issues during a free trial of our service
                - Internal users within our company(occasionally)
                    don't include any penalties
            - crafted by business and legal teams

        - SLO : Service Level Objectives
            - individual goals set for our system
            - represent the target values for the important quality attributes
            - Each SLO represents a target value/range that our service needs to meet
            - Example : availability service level objective of 3 nines
                        response time SLo less than 100 ms at 90th percentile
                        issue resolution between 24 to 48 hours
            - SLOs include:
                - quality attributes from the beginning of the design process
                - other objectives of our system
            - SLA aggregates all the SLOs in a single legal document
            - systems must have SLOs
                - as if we don't set SLOs, our users won't know what to expect from our system
            - If we can't meet the SLOs, we can't say that we meet our SLA
            - set by engineers

        - SLI : Service Level Indicators
            - quantitative measure of our compliance with a service-level objective
            - the actual numbers: 
                - measured using a monitoring service
                - calculated from our logs
            - can be later compared to our SLOs
            - example : % of requests that received successful response can be used as SLI for availability
            - set by engineers

        - Considerations:
            - shouldn't take all possible SLI's and define SLO associated with it
            - think about metrics that users care about the most
                from that, right SLI to track those SLO can be found
            - promising fewer SLO is better, commit to bare minimum
                - with many SLO's its hard to prioritize all of them equally
                - with few SLOs it easier to focus the entire architecture around them
            - Setting realistic goals with a budget for error
                - should commit to lower than we can provide
                - saves costs and incorporates unexpected issues
            - create a recovery plan for when teh SLI show that we are not meeting our SLO
                - need to decide ahead of time what to do if:
                    - the system goes down for long time
                    - performance degrades
                    - reports about issues/bugs in the system increases
                - plan should include:
                    - automatic alerts to engineers/DevOps
                    - automatic failovers/restarts/rollbacks/auto-scaling policies
                    - predefined handbooks on what to do in certain situations
    ```

### API design
#### Introduction to API Design
```
    - After capturing all functional requirements, we can think of our system as black box
        - The black box has:
            - behavior
            - well-defined interface : a contract between:
                - engineers : who implement the system
                - client apps : who use the system
            - since this interface is called by other apps, it is referred to as
                Application programming interface or API
            - in large-scale systems API is called by other apps remotely through
              the network
            - apps calling our API may be:
                - frontend clients like mobile apps/web browsers
                - backend systems that belong to other companies
                - internal systems within organization
            - each component of the system will have its own API
                - the component API's will be called by other apps within our system
    
    - API categories
        - Public APIs
            - exposed to general Public
            - any developer can use/call them from their application
            - Good practice:
                - requiring the users to register with us before allowing to send requests
                  and use the system
                  - allows control over who uses the system externally
                  - allows control over how they use the system
                  - beter security
                  - allows to blacklist users breaking rules
                
        - Private/Internal APIs
            - Exposed internally only within the company
            - They allow other teams/parts of the org to
                - take advantage of the system
                - provide bigger value for the company
                - not expose the system directly outside the organization

        - Partner APIs
            - similar to public APIs
            - Exposed only to companies/users having business relationship with us:
                - customer agreement after buying our product
                - subscibing to our service
        
    - Benefits of well designed API
        - client who uses it can immediately and easily enhance their business by using our system
        - the user of API need not know anything about our system's internal design/implementation
        - once we define and expose API, clients can integrate with us without waiting for full impl of our system
        - API makes it easier to design and architect the internal structure of our system
            - it defines endpoints to different routes that the user can take to use our system
    
    - API best practices and patterns
        - Complete encapsulation
            - of internal design and implementation
            - abstracting it away from a developer wanting to use our system
            - API should be completely decoupled from our internal design and implementation
            - this allows to change the design later without breaking the contract with clients
        - easy to use
            - easy to understand
            - impossible to misuse
            - only one way to get certain data/perform a task
            - descriptive names for actions and resources
            - exponsing only the information and actions that users need
            - keeping things consistent across all APIs
        - Keeping operations idempotent
            - An operation doesn't have any additional effect on the result if it is performed 
              more than once
              - example : updating the user's address to new address is idempotent
                          the result is the same regardless of performing in any number of times
                          increasing a user's balance by hundred dollars is not idempotent 
            - idempotent funcs preferred as APIs are going to be used through the network
            - if client app sends us a message:
                - the message can be lost
                - the response to that message may be lost
                - the message wasn't received as a critical component if our system went down
            - because of network decoupling, the client has no idea which scenario actually happened
            - if our operation is idempotent, they can simply resend the same message again without any consequences
        - API pagination
            - important when a response from our system contains very large payload/dataset
            - without pagination most client apps will:
                - not be able to handle big responses
                - result in a poor user experience
            - allows the client:
                - to request only a small segment of the response
                - specify the max size of each response from our system
                - specify an offset within the overall dataset
                - to receive the next segment increment the offset
            - client needs to provide number of items in response, and the starting point or offset
        - Asynchronous operations
            - some operations may need one big result at the end
            - example:
                - running a big report
                - big data analysis that scans a lot of records/log files
                - compression of large video files
            - if operation takes a long time, the client application has to wait for the result
              without making any progress
            - Async API : a client app receives a response immediately without having to wait for
                          the final result
                        the response includes some kind of identifier that allows:
                            - to track progress and status of the operation
                            - receive the final result
        - Versioning our API
            - Best API design allows us to make changes to internal design and implementation
              without changing the API
            - In practice, we may need to make non-backward compatible API changes
            - If we explicitly version the API we can:
                - Maintain two version of the API at the same time
                - Deprecate the older one gradually
                 
```

#### RPC : Remote Procedure Call
    ```
        - ability of client application to execute a subroutine on a remote server
        - the remote method invocation looks like calling a normal local method in terms of developer code
            - this is referred to as location transparency
        - RPC frameworks support multiple programming languages
        - API and the data types that are used in API methods are decared in a special
            Interface Description Language (IDL)(RPC framework dependent)
            essentially a schema definition of comm between remote client and server
            we can use schema to generate code using RPC code generation tool in multiple implemens
                one for client(client Stub), one for server(server stub)
            Datatypes are compiled into classes/structs depending on langauge, are referred to as Data Transfer Objects
        - RPC working :
            client---(response = rpc(arg1,arg2))--->client stub---(data serialization/marshaling)--->transport layer---
            (to the network)--->transport layer---(data unmarshalling)--->server stub---(call function)--->server
            similarly response is sent back from server to client
        - our job as API developers is:
            - to pick an appropriate framework
            - define the API and the relevant data types using IDL
            - publish the description
        - Pros
            - convenience to the devs of the client apps
            - they can communicate with our system easily by calling methods on
              objects similar to calling  normal, local  methods
            - the details of communication establishment or data transfer between clients to
              server are abstracted away from the developers
            - failures in communication with server result in an error/exception
        - cons
            -unlike local methods executed on client side, remote methods are:
                - slower
                - less reliable
            - client never knows how long remote method invocations can take
            - introduce async versions for slow methods
            - make operations idempotent when possible
        - when to use
            - used in communication between two backend systems
            - API provided to a different company instead of end user app/web page
            - communication between different components within a large scale system
            - abstracting away the network comm and focusing only on the actions the client
              wants to perform
        - when not to use:
            - when we don't want to abstract the network communication away
            - when we want to take direct advantage of HTTP cookies or headers
        - Importance:
            - revolves more around actions and less around data/resources
            - every action is a new method with a different name and signature
            - we can define many methods/actions without limitations
        - Other style of API can be better fit when:
            - desingning an API that is more data-centric
            - all operations needed are simple CRUD(Create, Read, Update, Delete)
            - example : REST apis
        - example frameworks for RPCs
            - gRPC (open source)
            - Apache Thrift
            - Java Remote method invocation(RMI)
    ```

#### REST API
    ```
        - Representational State Transfer
        - set of architectural constraints and best practices for defining APIs for the web
        - it is not a standard or protocol but an architectural style for designing API
          that are easy for our clients to use and understand
        - it makes it easy for a system to have quality attributes like :
            - scalability
            - high availability
            - performance
        - RESTful API: An API that obeys the REST architectural constraints
        - REST API style:
            - takes a more resource-oriented approach
            - main abstraction to the user is a named resource
            - the resources encapsulate different entities in our system
            - allows the user to manipulate those resources through small number of methods
        - client sends request for certain resource, server responds with current
            representation of the resource's state
        - commonly used protocol : HTTP
        - REST is dynamic in nature, while in RPC, the actions client can take regardless of its state
          are statically defined ahead of time in IDL
        - in restful API, this interface is a lot more dynamic through
            Hypermedia as the Engine of the Application State (HATEOAS)
            achieved by accompanying a state representation response to the client with
              hypermedia links
        
        - REST API quality attributes:
            - Statelessness:
                - server is Stateless, does not maintain any session info about client
                - each message should be served in isolation wihout any info about previous requests
                - achieves high scalability and availability
            - Cachebility
                - server has to either explicitly or implicitly define each response as:
                    - cacheable
                    - non cacheable
        
        - Named Resources
            - each resource is named and addressed using a URI(Uniform Resource identifier)
            - resources are organized in hierarchy
                - represented using '/'
            - each resource is either:
                - simple resource
                    - has a state
                    - can contain one/more sub-resources
                    - example : movie1
                - collection resource
                    - contains a list of resources of same type
                    - example : movies
            - representation of each resource state can be expressed in a variety of ways:
                - an image
                - a link to a movie stream
                - an object
                - an HTML page
                - Binary blob
                - executable code like javascript
        
        - Resources Best practices:
            - naming using nouns only
                - makes clear distinction from the actions that we're going to take
                - we're going to use verbs for the actions on those resources
            - make distinction between collection and simple resources:
                - plural names for collections, singular name for simple resources
            - give resources clear and meaningful names
                - ease of use for users
                - avoid overly generic names like: elements, entities, objects, items etc.
            - resource identifiers should be unique and url friendly
                - can be easily and safely used on the web

        - REST API operations
            - REST APIs limits the number of methods we can perform on each resource
            - predefined operations are:
                - Creating a new resource
                - Updating an existing resource
                - Deleting an existing resource
                - Getting the current state of the resource/list of sub-resources in case of collections
            - REST operations are mapped to HTTP methods as follows:
                - create : POST
                - update : PUT
                - Delete : DELETE
                - Get/List : GET
                - in some situations we define custom methods
            - HTTP methods guarantees
                - GET methods is considered safe, applying it to a resource would not change its state
                - GET, PUT, DELETE are idempotent
                - GET requests are considered cacheable by default
                - responses to POST requests can be made cacheable
            - To send additional information to the system as part of POST or PUT use:
                - JSON
                - XML
        
        - REST API Definition steps:
            Example : movie streaming service
            1. Identifying entities
                - users, movies, reviews, actors
            2. Mapping entities to URIs
                - users
                    - /users
                    - /users/{user-id}
                - movies
                    - /movies
                    - /movies/{movie-id}
                - actors 
                    - /actors
                    - /actors/{actor-id}
                - Reviews
                    - /movies/{movie-id}/reviews
                    - /movies/{movie-id}/reviews/{review-id}
            3. Defining Resources' Representations
                - most commonly represented in JSON format
                - {"movies":[{"name":"AI", "id":"movie-123"},...  ]}
                - we can represent resource as resource info + links(containing actions that can be performed on resource)
            4. Assigning HTTP methods to operations on resources
                - POST /users => create new user
                    - response will contain newly created user-id
                - GET /users/{user-id} => - get user information
                - PUT /users/{user-id} => update User information

    ```

### Large Scale Systems Architectural Building Blocks

#### DNS, Load Balancing and GSLB
    ```
        - Load Balancers
            - balance the load among a group of servers in a system
            - make sure no one server is overloaded
            - provide abstraction for client application, a single server view to client app
            - quality attrbitutes provided :
                - scalability
                    - auto scaling is available in cloud environment which automatically add/remove servers based on:
                        - requests per second
                        - network bandwidth etc..
                - High availability
                    - load balancer can act as monitoring service
                - Performance
                    - may increase delay, but throughput is increased
                - Maintainablity
            - types:
                - DNS load Balancing
                    - DNS maps human-friendly urls to IP addresses
                    - DNS can return list of IP addresses in some meaningful order
                    - client picks the first IP in the list
                    - rotating list in round robin fashion
                    - cons:
                        - DNS doesn't monitor health of the servers
                        - list of IP addresses change based on TTL configured for that particular DNS records
                        - list of addresses can be cached in different locations such as client
                        - always simple round robim used
                        - loss of abstraction and makes sysem less secure
                - Hardware load balancer
                    - run on dedicated devices designed specifically for load Balancing
                - software load balancer
                    - can run on any general purpose computer
                    - can monitor the servers
                    - create abstraction for the client
                        - multiple servers can be used for multiple services
                    - load balancer should be placed close to the main servers
                        - for global apps it can be bad
                    - load balancers on their own do not solve the DNS resolution problem
                - Global Server Load Balancer(GSLB)
                    - hybrid of DNS and hw/sw load balancer
                    - can make intelligent routing decisions
                    - solves locally required load balancer problem
                    - good for global scale apps like Data centers
                    - can be configured to route users based on :
                        - current traffic
                        - CPU load in each data center
                        - Best estimated response time
                        - Bandwidth between user and data center
                    - very helpful in disaster recovery
                    - provide DNS load balancing
            - Load balancing solutions & cloud techs:
                - HAProxy - open source
                - NGINX - open source
                - AWS - Elastic Load Balancing (ELB)
                - Google cloud platform - Cloud Load balancing
                - Microsoft Azure load balancers
                - Amazon Route 53 (GLSB)
                - AWS Global Accelerator (GLSB)
                - GCP load balancer & cloud DNS(GLSB)
                - Azure Traffic Manager (GLSB)

    ```

#### Message Brokers
    ```
        - basic building block for asynchronous software architecture
        - uses queue data structure to store messages between senders and receivers
        - used internally and not exposed externally
        - basic capabilities:
            - storing/ temporarily buffering the messages
                - absorbs traffic spikes
            - Message routing
            - Transformation validation
            - load balancing
        - entirely decouples senders from receivers by using their own protocols and apis
        - offer publish/subscribe pattern where services can:
            - publish a message to a particular channel
            - subscribe to that channel
            - get notified when a new event is published
            - can add multiple servers without any modification to the system
        - Quality attributes:
            - high Fault tolerance : prevents message loss 
            - high availability and scalability
            - performance
                - increases latency, not too significant for most systems
        - examples : 
            - Apache Kafka - open source
            - RabbitMQ - open source
            - cloud based message brokers
                - Amazon Simple Queue Service
                - GCP Pub/Sub and Cloud Tasks
                - Microsoft Azure
                    - service bus, event hubs, event grid
    ```

#### API Gateway
    ```
        - one service codebase for large application can become too complex
            so we split the codebase into multiple services and servers
            - this causes to split single API into multiple APIs
            - so now we need to update the frontend to be aware of the internal org of the system
            - users make calls to different services on browsers depending on the task
            - so each server needs to implement security and other things
        - to avoid the above redundancy, API gateway is used
        - API management service that sits in between the clients and collection of backend services
        - follows a sw arch pattern called API composition
            - compose all the APIs of all our services into one single API
            - client apps can call one single service
        - provides abstraction for clients
        - benefits:
            - seamless internal modification/refactoring
            - consolidating all security/authorization and authentication in one single place
            - implement rate-limiting to prevent DoS attacks
            - request routing : can save users from making multiple requests to different services
                - user makes single request, API gateway calls as many required and aggregate into one single response
                    which gets returned to the user
            - static content and response caching
            - monitoring and alerting
            - protocol translation in a single place
        - considerations and anti-patterns
            - API gateway shouldn't contain any business logic
            - API gateway may become single point of failure
                - multiple instance behind load balancer
                - eliminate possiblility of human error
                - release changes to gateway with extreme caution
            - avoid bypassing API gateway from external services
        - examples :
            - Netflix Zuul (open source: Java)
            - cloud based:
                - Amazon API gateway
                - GCP API gateway
                - Microsoft Azure API management
    ```

#### Content Delivery Network - CDN
    ```
        - helps in reducing network latency in case of global level apps
        - 53% of mobile users abandoned a website if it took longer than 3 seconds to load
        - we need to get larger static content like :
            - images, HTML pages, javascript, CSS files, Videos
            closer to the user to get faster load times
        - CDN : Globally distributed network of servers located in strategic places
        - Main purpose is to speed up the delivery of content to end-users
        - CDN were created to solve the problem referred to as Wide World Wait
          which means bad user experience caused by: 
            - slow internet connection
            - overloaded web server
        - CDN provide service by caching our website content on their edge servers
          which are relocated at different Points of Presence(PoP) which are:
            - physically closer to the user
            - more strategically located in terms of network infrastructure
        - this allows them to transfer content much quicker and improver perceived system performance
        - CDNs can be used to deliver:
            - images, text, css, js files, video streams(live and on-demand)
        - use cases:
            - E-comm, financial institutions, tech and software as a service companies
            - media companies delivering videos/news, social media
        - Quality attrbitutes:
            - Performance : faster page loads
            - high availability : issues/slowness are less noticable
            - security : protection against DoS
        - additional techinques:
            - use faster and more optimized hard drives to store cached content
            - reduce bandwidth by compressing the content delivered using algos like:
                - gzip
                - minification of js files
        - Content publishing strategies:
            - pull strategy
                - we need to tell the Content Delivery network provider:
                    - which content we want on our website cached
                    - how often this cache needs to be invalidated
                - configured by setting a time to live(TTL) property to each asset/type of asset
                - first time there is no cache on CDN, CDN pulls on first request
                - pros 
                    - lower maintenance on our part
                - cons
                    - first user will have longer latency
                    - setting same TTL for all assets may cause load spikes on our servers
                    - need to maintain high availability on our system
            - Push strategy
                - manually or automatically publish content to edge servers
                - pros
                    - push once on CDN if content doesn't change frequently
                - cons
                    - if content changes frequently, need to maintain regularly
        - Examples:
            - Cloudflare
            - Fastly
            - Akamai
            - Amazon CloudFront
            - GCP CDN
            - MS Azure CDN
    ```

### Data Storage At global Scale

#### Relational DB and ACID transactions
    ```
        - data stored in tables
        - structure(schema) is always defined ahead of time
        - robust query language : SQL
        - allows to eleminate the need for data duplication
        - Pros:
            - ability to form complex and flexible queries
            - efficient storage
            - natural structure of data for humans
            - ACID transactions
                - transcation is a sequence of operations that for an external
                  observer should appear as a single operation
                - Atomicity
                    - each set of operations appear all at once or nothing at all
                - Consistency
                    - already commited transaction will be seen by all future queries/transactions
                    - makes sure that a transaction doesn't violate any constraint that we set
                - Isolation
                    - related to Atomicity in context of concurrent operations performed on our DB
                    - concurrent transactions will not see intermediate states of the DB
                - Durability
                    - once transaction complete, its final state will persist and remain
                      permanently inside the DB
        - Cons:
            - Rigid structure enforced by table's schema
            - hard to maintain/scale
            - slower read operations
        - When to use:
            - perform complex and flexible queries to analyze our data
            - need to guarantee ACID
        - When to use:
            - there isn't any relationship between different records
            - read performance is most important quality that we need for providing good user experience
    ```
#### Non-Relational Databases /NoSQL DBs
    ```
        - solve drawbacks of relational databases
        - allow to logically group a set of records without forcing all to have same structure
        - easy to add additional attributes without affecting other entries
        - don't store data in tables
        - support more native data structures to programming languages
            - eliminates need for an ORM (object relational Mapping)
        - designed for faster queries
        - lose the ability to easily analyze, join operations on the records
        - ACID transactions are rarely supported
        - Categories :
            - Key/Value Store
                - essentially large-scale hashtable
                - useful for caching/counting
            - Document store
                - allows for storing documents with little bit more structure
                - each document can be an object with different attributes of different types
                    - JSON/YAML/XML etc.
            - Graph DB
                - extension of a document store with additional capabilities to:
                    - link, traverse, analyze multiple records more efficiently
                - optimized for navigating and analyzing relationships between different records
                - use cases
                    - fraud detections
                    - recommendation engines
        - How to choose:
            - analyze use case
            - figure out which properties are most important and which can be compromised on
        - When to choose DB:
            - perfect for fast queries and caching
            - handling real-time big data
            - data is not structured
            - examples :
                - user profiles
                - content management
        - Examples:
            - Key/value stores
                - Redis
                - Aerospike
                - Amazon DynamoDB
            - Document Store
                - Cassandra
                - MongoDB
            - Graph DB
                - Neo4j
                - Amazon Neptune
     
    ```

#### Techniques to improver performance, availability and scalability of Databases
    ```
        - Database Indexing
            - speeds up retrieval operations
            - locate the desired records in sublinear time
            - without indexing ops may require full table scan which is inefficient
            - index table is a helper table created from a particular column or group of columns
            - once index table is created we can put it inside a DS like:
                - hashmap
                - self-balanced Tree(B-Tree)
            - composite index : formed from set of columns
            - read queries are faster in the expanse of:
                - additional space for index tables
                - speed of write operations
            - extensively used in NoSQL DBs
        
        - Database Replication
            - DB server may become a single point of failure 
            - create multiple instances of DB server to increase fault tolerance
            - higher complexity in operations like:
                - write, update, delete
            - not trivial to make sure that concurrent modifications on same record:
                - don't conflict with each other
                - provide guarantees in terms of consistency and correctness
            - distributed DBs
                - difficult to design, configure and manage on large scale
                - require competency in the field of distributed systems
            - supported by all modern DBs
                - Non-relational DBs : incorporate replication out of the box
        
        - Database Partitioning / Sharding
            - split data among different DB instances
            - we can scale our DB to store more data
            - different queries can be performed in parallel
            - turns DB into distributed DB
            - need to route queries
            - first-class feature in Non-relational DB
            - less supported on relational DBs
            - for high volume of data, definitely required
            - infrastructure Partitioning
        
        - all above techniques are orthogonal and can be used together
    ```

#### Eric Brewer's (CAP) Theorem
    ```
        - In the presence of network partition, a distributed DB cannot guarantee
          both consistency and availability and has to choose only one of them.
        - forces our DB to a choice only when there is network partition
        - majority of time we can easily provide both consistency and availability when:
            - there is no network partition
            - replicas can freely communicate with each other
        - Consistency Availability Partition Tolerance
            - consistency : every read request receives either the most recent write or an error
            - Availability : every request receives a non-error response without the guarantee that
                it contains the most recent write
            - Partition Tolerance : system continues to operate despite an arbitrary number of messages
                begin lost or delayed by the network between different computers
        - CAP theorem tells us that when we choose or configure a db we have to drop
          one of the above three properties:
            - CA : no partition tolerance   
                - not scalable and no fault tolerance
                - centralized
            - CP : no availability
                - example : items in cart
            - AP : no consistency
                - example : likes on a post
        - important in Software Architecture to integrate quality attributes required by the system
        - tradeoff between consistency and availability

    ```

#### Scalable Unstructured Data Storage    
    ```
        - Unstructured data
            - data that doesn't follow a particular structure, schema or model
            - binary files like audio, video, image, pdfs
            - blob : binary large objects
            - dbs are optimized for structured data and impose size limit on Unstructured data
            - use cases:
                - allow upload data to server for further processing(compress/stream/backup)
                - backup and archiving snapshots of DBs for data recovery
                - web hosting : media content like images/thumbnails
                - Machine learning and Big Data analytics : sensor data collection etc
                - data sets are very big
                - each file/object is very big
        
        - Distributed File System (DFS)
            - provides same abstraction as data is stored on same harddrive
            - network of storage devices connected to each other
            - provides replication, strong/eventual consistency and autohealing
            - binary objects stored in a familiar way in form of file in a tree like structure
            - pros:
                - no need for special API to access files
                - easy to modify the files if needed
                - very efficient and high performance IO operations
                    - useful for ML and IOT projects
            - cons:
                - number of files is limited
                - no easy access through web API(HTTP + REST)
                - if versioning required, have to use external version control system

        - Object Store
            - Scalable storage solution for storing Unstructured data at internet scale
            - objects not stored in tree like structure
            - objects stored in containers called buckets
            - Object store abstraction: an Object with Name/id, metadata(key value pairs), value(content), access control list
            - use data replication
            - pros:
                - linear scalability
                - no limit to the number of objects we can store
                - very high limit on single object size
                - provides HTTP + REST API
                - supports for versioning out of the box
            - cons:
                - objects are immutable
                    - can only replace an existing object with a new version
                - no easy file system-like access
                    - access through SDK or REST API
                - Lower IO performance compared to a DFS 
            - Examples : 
                - cloud based:
                    - Amazon S3
                    - GCP storage
                    - Azure Blob
                    - Alibaba OSS
                - open source:
                    - OpenIO
                    - MinIO
                    - Ceph

    ```

### Software Architecture Patterns and Styles
#### Introduction
    ```
        - Software Architectural Patterns
            - General repeatable solutions to commonly occuring system design problems
            - common solutions to sw arch problems that involve multiple components that run as seperate runtime units
            - incentives
                - save valuable time and resources
                - avoid making our architecture resemble big ball of mud(anti-pattern) : tightly coupled, global data etc..
                - other engineers/software architects can follow it
            - all software architectural patterns are just guidelines
            - as system evolves, certain patterns may not fit us anymore
            - we would need to migrate to different architectural pattern that now fits us better
    ```

#### Multi-Tier Architecture
    ```
        - organizes our sytem into multiple physical and logical tiers
        - Logical seperation : limits the scope of responsibility on each tier
        - Physical seperation : allows each tier to be seperately:
            - developed
            - upgraded
            - scaled
        - different from multilayer architecture which is internal seperation inside an application into multple layers
        - restrictions :
            - each pair of applications that belong to adjacent tiers communicate each other using client/server model
            - discourages communication that skips through tiers
                - keeps tiers loosely coupled
        
        - three tier/monolithic architecture:
            - most common and popular pattern for client-server, web-based services
            - First tier : Presentation Tier
                - contains the user interface
                - display info to the user
                - take the user's input through GUI
                - example : webpage, mobile app, desktop GUI app
                - does not contain any business logic
            - Middle/Second tier: Application/Logic/Business Tier
                - provides all the functionality and the features(functional requirements)
                - processing the data that comes from the presentation tier
                - applying the relevant business logic to it
            - Last tier : Data tier
                - responsible for storage and persistence of user and business specific data
                - may includes files and definitely contains databases
            - Reasons for popularity:
                - fits a large variety of use cases
                    - example : online store, new website, video/audio streaming service
                - easy to scale horizontally to:
                    - handle large traffic
                    - handle a lot of data
            - drawback:
                - monolithic structure of logic tier
                    - no business logic in presentation or data tier
                    - all business logic in single codebase that runs in
                      single runtime unit
                    - high CPU and memory consumption
                        - makes application slower and less responsive
                        - may require upgrading using vertical scaling
                    - low development velocity
                        - large complex codebase harder to:
                            - develop, maintain, reason about
                        - can be somewhat improved by:
                            - logically splitting applications codebase into separate modules
                    - organization scalability is limited
            - perfect choice for:
                - relatively small and not complex codebase
                - maintained by small team of devs
                - early-stage startups or well established companies
                - if we don't need to split our codebase into multiple components

        - two tier architecture:
            - Presentation + business tier
            - Data tier
            - eliminates the overhead of middle logic layer
            - provides faster more native experience to the users
            - examples:
                - desktop/mobile versions of editors for
                    - document, image, music
        
        - four tier:
            - presentation
            - API gateway
            - application
            - data

        - avoid more that four tiers to avoid tight coupling and skipping tiers
    ```

#### Microservices Arhitecture
    ```
        -  organizes our business logic as a collection of loosely coupled and independently deployed services. Each service is owned by a small team and has a narrow scope of responsibility
        - advantages
            - smaller codebase
                - development becomes faster and easier
                - building and testing is faster and easier
                - troubleshooting/adding new features is easier
                - new developers can become fully productive a lot faster
            - better performance and scalability
                - instances become less CPU instances and less memory intensive
            - better organizational scalability
            - better security through fault isolation
        - considerations and best practices:
            - we don't get all benefits out of the box
            - if we don't follow we can fall into big ball of mud
            - overhead and challenges
                - organizational decoupling : we need to make sure that the services are logically separated such that every change:
                    - can happen only in one service 
                    - would not involve multiple teams
            - Best practices:
                - Single Responsiblity principle
                    - each service needs to be responsible for only one:
                        - business capability
                        - domain
                        - resource
                        - action
                - seperate db for each service
        
        - always start with simple monolithic approach first
          when monolithic architecture stops working, we should consider microservices

    ```

#### Event Driven Architecture
    ```
        - synchronous communication in microservice architecture
            - a service needs to know about other service
            - needs to have api reference and calls the api synchronously
            - there is dependency because of this between microservices
        - Event Driven Architecture:
            - there are no direct messages/requests that ask for data
            - there are only events
            - Event: an immutable statement of a fact or a change
                - Fact event Example: user clicking on a digital ad
                - Change events: player of a video game, iot device
            - Components:
                - Event Emitter
                - message broker
                - event consumer
        - Benefits when used with microservice architecture
            - decoupling of microservices using events and message brokers
            - asynchronous message passing
            - higher scalability
            - more services can be added to the system without any changes
            - allows :
                - analyze streams of data
                - detect patterns and act upon data in real-time
        - Event Sourcing Pattern:
            - instead of storing data in db, we can simply store the events and replay the events whenever necessary to analyze current state
            - eliminate need for database
            - append event to log whenever they come
            - can store events for as long as we can
            - add snapshot events to make querying faster
        - CQRS (Command Query Responsibility Segregation)
            - solves:
                - optimizing a DB with high load of read and update operations
                    - concurrent read and update make operations slow
                    - we can optimize only read/write while compromising other
                - joining multiple tables located in separate DBs that belong to different microservices
            - allows to separate update and read operations into seperate dbs
              sitting behind two different services
            - a separate service programatically joins data from two services which are using different dbs, using the events published by the services, and stores the result in read-only DB

    ```

### Big Data Architecture Patterns

#### Intro to Big Data
    ```
        - Big Data are datasets that are either:
            - too large in size
            - too complex in structure
            - Come to our system at a high rate that exceeds the capacity of a traditional application
        - Characteristics of Big Data:
            - Volume:
                - refers to the quantity of data that we need to process, store, analyze
                - large quantities of data (~terabytes/petabytes per day)
                - examples : internet search, medical software, real time security, weather prediction
            - Variety
                - large variety of Unstructured data from multiple sources
                - data fusion : combining data together
                    - helps in finding patterns and insights for our org
                - example : social media data collection: likes, ad views, clicks etc.
            - velocity
                - continuous stream of data that comes to our system at a high rate due to:
                    - large scale of the system
                    - high frequency of events
                - example: millions of users using an online store will cause very high frequency of events
                           iot system with lots of sensors on many devices
        - Purpose of processing big data:
            - insights from analyzing big data:
                - provide significant advantage over competition
                - come in form of visualization, querying capabilities, predictive analysis
                - helps in building algos or machine learning models or predict potential failures from log analysis
    ```

#### Big Data Processing Strategies
    ```
        - Batch Processing:
            - store incoming data into distributed database or distributed file system
            - data is never modified, only appended
            - do not process each piece of data individually, but as batches of records
            - perform processing on fix schedule(like once a day/hour/month)
            - batch processing performs the analysis and produces up-to date view of the data we have
                which can be stored in structured database which can be queried
            - the view generated should reflect the knowledge we have about entire dataset
            - depending on the use case batch processing can:
                - pick up only the recently arrived data
                - process entire data set from scratch
            - Example :
                - online learning subscription platform
                    - ~1million events/minute due to video watching progress & reviewing etc
                - search engine service
                    - daily web crawler => index data
            - advantages:
                - easy to implement
                - high availability
                - better efficiency
                - high fault tolerance towards human error
                - complex and deep analysis of large datasets
            - cons:
                - long delay between incoming data and result from the processing job
                - no real time view of data coming in
        
        - Real time processing:
            - each event put onto the message queue which feeds the stream processing job
            - advantages:
                - analyze and respond to data as it comes into our system immediately
            - cons:
                - very hard to do any compex analysis in real time
                - doing data-fusion/ analysing historical data is almost impossible
            - Example:
                - log analysis
                - stock market app
    ```

#### Lambda Architecture
    ```
        - in many cases we need properties of both real time analysis and batch processing
            - like ride sharing service may need both real time data(to match driver and clients) and
              historic data(for finding busy times in a day etc)
        - Lambda architecture provides properties of both
        - attempts to find balance between:
            - high fault tolerance and comprehensive analysis of data(Batch Processing)
            - low latency(real-time processing)
        - infra is divided into 3 layers: the incoming data is passed into batch and speed layer simultaneously
            - Batch layer:
                - manage dataset and be a system of records
                - precompute batch views
                - aims at perfect accuracy and operates on entire dataset
            - speed layer
                - real time processing happens here
                - processing job analyze incoming data to real-time view
                - compensates for the high latency in batch layer
                - operates only on recent data
            - serving layer
                - respond to ad-hoc queries
                - merge data from batch layer and speed layer to return required view
        - Example:
            - Ad tech companies:
                - events:
                    - user saw ad
                    - user clicks ad
                    - user makes a purchase from ad
    ```
    