# Suse Cloud Native scholarship Lesson 2

## Architecture Considerations for Cloud Native Applications

### Design Considerations

When building an application it is important to allocate time for context discovery. This includes listing all necessary functionalities of the application and enumerating any resources that can enable its buildout. This phase sets the fundamentals of the project. If properly implemented, it can enable the creation of services that are scalable, resilient, and extensible.

The first step in the context discovery process is to list the functional requirements, or what application capabilities should deliver to the end-users. For example, a good starting point is to expand on the following:

- Stakeholders
- Functionalities
- End users
- Input and output process
- Engineering teams

The second step is to enumerate the available resources that facilitates the implementation of the project. For example, a good starting point is to list available:

- Engineering resources
- Financial resources
- Timeframes
- Internal knowledge

Having a good understanding of functional requirements and available resources can lead to a simpler choice between monolithic and microservice-based architectures.

### Monoliths and Microservices

Prior to an organization delivers a product, the engineering team needs to decide on the most suitable application architecture. In most of the cases 2 distinct models are referenced: monoliths and microservices. Regardless of the adopted structure, the main goal is to design an application that delivers value to customers and can be easily adjusted to accommodate new functionalities.

Also, each architecture encapsulates the 3 main tires of an application:

- UI (User Interface) - handles HTTP requests from the users and returns a response
- Business logic - contained the code that provides a service to the users
- Data layer - implements access and storage of data objects

### Monoliths

In a monolithic architecture, application tiers can be described as:

- part of the same unit
- managed in a single repository
- sharing existing resources (e.g. CPU and memory)
- developed in one programming language
- released using a single binary


Imagine a team develops a booking application using a monolithic approach. In this case, the UI is the website that the user interacts with. The business logic contains the code that provides the booking functionalities, such as search, booking, payment, and so on. These are written using one programming language (e.g. Java or Go) and stored in a single repository. The data layer contains functions that store and retrieve customer data. All of these components are managed as a unit, and the release is done using a single binary.


### Microservices

In a microservice architecture, application tiers are managed independently, as different units. Each unit has the following characteristics:

- managed in a separate repository
- own allocated resources (e.g. CPU and memory)
- well-defined API (Application Programming Interface) for connection to other units
- implemented using the programming language of choice
- released using its own binary

Now, let's imagine the team develops a booking application using a microservice approach.

In this case, the UI remains the website that the user interacts with. However, the business logic is split into smaller, independent units, such as login, payment, confirmation, and many more. These units are stored in separate repositories and are written using the programming language of choice (e.g. Go for the payment service and Python for login service). To interact with other services, each unit exposes an API. And lastly, the data layer contains functions that store and retrieve customer and order data. As expected, each unit is released using its own binary.

### New terms

*Monolith*: application design where all application tiers are managed as a single unit
*Microservice*: application design where application tiers are managed as independent, smaller units

### Further reading
[What’s the Difference Between Monolith and Microservices?](https://nordicapis.com/whats-the-difference-between-monolith-and-microservices/)
[Microservices vs Monolithic Architecture](https://www.mulesoft.com/resources/api/microservices-vs-monolithic)

## Trade Offs

### Development Complexity

Development complexity represents the effort required to deploy and manage an application.

- *Monoliths* - one programming language; one repository; enables sequential development
- *Microservice* - multiple programming languages; multiple repositories; enables concurrent development

### Scalability

Scalability captures how an application is able to scales up and down, based on the incoming traffic.

- *Monoliths* - replication of the entire stack; hence it's heavy on resource consumption
- *Microservice* - replication of a single unit, providing on-demand consumption of resources

### Time to Deploy

Time to deploy encapsulates the build of a delivery pipeline that is used to ship features.

- *Monoliths* - one delivery pipeline that deploys the entire stack; more risk with each deployment leading to a lower velocity rate
- *Microservice* - multiple delivery pipelines that deploy separate units; less risk with each deployment leading to a higher feature development rate

### Flexibility

Flexibility implies the ability to adapt to new technologies and introduce new functionalities.

- *Monoliths* - low rate, since the entire application stack might need restructuring to incorporate new functionalities
- *Microservice* - high rate, since changing an independent unit is straightforward

### Operational Cost

Operational cost represents the cost of necessary resources to release a product.

- *Monoliths* - low initial cost, since one code base and one pipeline should be managed. However, the cost increases exponentially when the application needs to operate at scale.
- *Microservice* - high initial cost, since multiple repositories and pipelines require management. However, at scale, the cost remains proportional to the consumed resources at that point in time.

### Reliability

Reliability captures practices for an application to recover from failure and tools to monitor an application.

- *Monoliths* - in a failure scenario, the entire stack needs to be recovered. Also, the visibility into each functionality is low, since all the logs and metrics are aggregated together.
- *Microservice* - in a failure scenario, only the failed unit needs to be recovered. Also, there is high visibility into the logs and metrics for each unit.

Each application architecture has a set of trade-offs that need to be considered at the genesis of a project. But more importantly, it is paramount to understand how the application will be maintained in the future e.g. at scale, under load, supporting multiple releases a day, etc.

There is no "golden path" to design a product, but a good understanding of the trade-offs will provide a clear project roadmap. Regardless if a monolith or microservice architecture is chosen, as long as the project is coupled with an efficient delivery pipeline, the ability to adopt new technologies, and easily add features, the path to could-native deployment is certain.

## Best Practices for Deployment

### Heath Checks

Health checks are implemented to showcase the status of an application. These checks report if an application is running and meets the expected behavior to serve incoming traffic. Usually, health checks are represented by an HTTP endpoint such as ` /healthz` or` /status`. These endpoints return an HTTP response showcasing if the application is healthy or in an error state.

### Metrics

Metrics are necessary to quantify the performance of the application. To fully understand how a service handles requests, it is mandatory to collect statistics on how the service operates. For example, the number of active users, handled requests, or the number of logins. Additionally, it is paramount to gather statistics on resources that the application requires to be fully operational. For example, the amount of CPU, memory, and network throughput. Usually, the collection of metrics are returned via an HTTP endpoint such as `/metrics`, which contains the internal metrics such as the number of active users, consumed CPU, network throughput etc.

### Logs

Log aggregation provides valuable insights into what operations a service is performing at a point in time. It is the nucleus of any troubleshooting and debugging process. For example, it is essential to record if a user logged in successfully into a service, or encountered an error while performing a payment.

Usually, the logs are collected from STDOUT (standard out) and STDERR (standard error) through a passive logging mechanism. This means that any output or errors from the application are sent to the shell. Subsequently, these are collected by a logging tool, such as Fluentd or Splunk, and stored in backend storage. However, the application can send the logs directly to the backend storage. In this case, an active logging technique is used, as the log transmission is handled directly by the application, without a logging tool required.

There are multiple logging levels that can be attributed to an operation. Some of the most widely used are:

- DEBUG - record fine-grained events of application processes
- INFO - provide coarse-grained information about an operation
- WARN - records a potential issue with the service
- ERROR - notifies an error has been encountered, however, the application is still running
- FATAL - represents a critical situation, when the application is not operational

As well, it is common practice to associate each log line with a *timestamp*, that will exactly record when an operation was invoked.

### Tracing

Tracing is capable of creating a full picture of how different services are invoked to fulfill a single request. Usually, tracing is integrated through a library at the application layer, where the developer can record when a particular service is invoked. These records for individual services are defined as spans. A collection of spans define a trace that recreates the entire lifecycle of a request.

### Resource Consumption

Resource consumption encapsulates the resources an application requires to be fully operational. This usually refers to the amount of CPU and memory that is consumed by an application during its execution. Additionally, it is beneficial to benchmark the network throughput, or how many requests can an application handle concurrently. Having awareness of resource boundaries is essential to ensure that the application is up and running 24/7.
