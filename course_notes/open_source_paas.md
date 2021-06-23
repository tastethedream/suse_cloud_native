# Lesson 4: Open Source PaaS


## Cluster Management

Kubernetes offers a wide range of functionalities that form the principles of any modern infrastructure. These capabilities revolve around automation, portability, extensibility, flexibility, self-healing, and many more. However, managing Kubernetes at scale is challenging, especially when the clusters are self-hosted in datacenters or private clouds. In this case, a team has to keep up-to-date with the latest Kubernetes releases, to ensure the platform is updated, upgraded, managed, and configured to meet the production-grade standards.

Within any organization, a release process consists of releasing the application to multiple environments, such as sandbox, staging, and production. In this case, each environment is represented by a separate Kubernetes cluster, with a total of 3 clusters. However, the number of clusters grows exponentially if the platform is replicated across different regions. This is usually required to keep market proximity and deliver a service to consumers with minimal latency. As a result, if the infrastructure is distributed across the US, Europe, and the Asia Pacific, a team ends up operating 9 clusters. Configuring, managing, upgrading, updating, and deploying to all of these clusters is a herculean task and often requires a dedicated team.

In these circumstances, if an organization does not have sufficient engineering resources, delegating the platform management to a 3rd party provider is a more suitable solution. This is covered by a PaaS or Platform as a Service solution.

## PaaS Mechanisms

- *On-premise* - where an engineering team has full control over the platform, including the physical servers
- *IaaS* or *Infrastructure as a Service* - where a team consumes compute, network, and storage resources from a vendor
- *PaaS* or *Platform as a Service* - where the infrastructure is fully managed by a provider, and the team is focused on application deployment

Releasing a product to a production environment implies that a platform has been build to host this particular product. A platform consists of multiple services that need to be configured, wired, and maintained together. These services are:

- *Networking* - establish communication between internal and external systems, such as internet connection, firewalls, routers, and cables

- *Storage*- collect and store digital data, such as files, blocks, or objects

- *Servers* - physical machines that provide compute services for a platform

- *Virtualization* - abstracts physical servers, storage, and networking. For example, we have learned that hypervisors are used to virtualize servers.

- *O/S* - operating systems that connect the software to physical resources (e.g. Linux, Ubuntu, Windows, etc)

- *Middleware* - help the developers to build an application by making it easy to consume platform capabilities (e.g. messaging, API, data management)

- *Runtime* - execution context for an application. For example, using JVM (or Java Virtual Machine) as a Java runtime

- *Data* - tools for collection and storage of data that is required by an application during execution(e.g. MySQL, MongoDB, or CockroachDB)

- *Applications* - the business logic for a product

### On-premise

On-premise represents a cloud-computing offering where the engineering team has full control of the platform services (from networking to applications). This solution is suitable for organizations that have sufficient engineering power and regulations that demand full control of their technology stack and operations within it.

### IaaS

IaaS solutions provide the abstraction of networking, storage, server, and virtualization layers. As a result, these services are consumed on-demand by the engineering teams. Additionally, IaaS provides a suitable abstraction for the management of self-hosted Kubernetes clusters, which depend on compute, network, and storage components for a successful bootstrap process.

The most common IaaS solutions are delivered by public cloud providers such as AWS, GCP, Microsoft Azure, and many more.

### PaaS

PaaS is a cloud-computing offering that enables an engineering team to fully focus on application development. It abstracts all services except the application and the data associated with it. As a result, the team is required to manage the code base and any database service that the product needs to be fully operational.

Popular PaaS solutions are App Engine from GCP, Heroku, Cloud Foundry, Beanstalk from AWS, and many more.

#### Advantages:

- *Time-efficiency* - engineering focus is shifter toward development rather than infrastructure management

- *Scalability and high availability* - on-demand resource consumption enables an application to easily scale and fail-over

- *Rich application catalog* - integration of external service (e.g. databases) with minimal effort

#### Disadvantages:

- *Vendor lock-in* - it is challenging to interchange PaaS providers without service disruption

- *Data security* - since data is managed by a 3rd party, an extra layer of complexity is added to ensure data confidentialit

- *Operational limitations* - the service catalog is limited to the services offered by the integrated cloud provider

Throughout its evolution, an organization might use one or a subset of available cloud-computing services. It is essential to select an offering that closely meets business requirements. However, it is important to consider the following traits of cloud-computing services:

- The fewer components are delegated to external providers, the more control there is over available functionalities

- The more ownership there is across the stack, the more complexity is introduced in managing and delivering the product

- The fewer components are managed by an engineering team, the quicker is the usability of the stack. As such, with a PaaS offering the engineering team can deploy their application immediately. While if choosing an on-premise solution, the release of a product is possible only after the platform is built.

By default, the PaaS solutions offer the management of the underlying infrastructure, such as storage, databases, compute, hosting, and many more. Also, the majority of solutions will provide data analytics, security, and advanced scheduling.

As such, the web-store project will benefit from the following PaaS capabilities:

- database - for storing the customer details, orders, and products details
- compute - accessible scalability for the application to serve millions of customers
- networking - hosting and serving the requests with no downtime
- analytics - an add-on to collect data and provide metrics and logs about customer interaction with the web-store
