# Lesson 4 Open Source PaaS

## Cloud Foundry

Integrating PaaS solutions within an organization shifts the engineering effort from infrastructure management to product development. Additionally, it provides a powerful developer experience throughout the release process of an application, where the main functionalities are consumed through a UI or console. However, there is one major downside with this offering: vendor and application catalog lock-in. If an application is deployed using a PaaS offering from GCP, the application catalog of external services is narrowed down to GCP services mainly. Consequently, the open-source fundamentals were applied to PaaS offerings, resulting in an open-source PaaS such as Cloud Foundry.

Cloud Foundry is an open-source PaaS, a stand-alone software package that can be installed on any available infrastructure; private, public, or hybrid cloud. Due to its open-source nature, there is no vendor lock-in and the community can contribute to its future releases and definition of the roadmap. As such, Cloud Foundry offers a production-grade release process through a solid developer experience, that enables any application to be deployed with ease.

Note: some offerings of Cloud Foundry, can be deployed on top of Kubernetes, meaning that its main components are running as pods within a cluster.

Cloud Foundry consists of multiple components that provide these core capabilities:

 - *Routing* - handle the incoming external traffic and route it to applications

 - *Authentication* - identity management to user accounts

- *Application lifecycle* - controls the application deployments, monitors their status, and reconciles any new changes to reach the desired state of the application.

 - *Application storage and execution* - handle the availability of artifacts to applications

- *Service* - use service brokers to provisions on-demand the dependency services for an application, such as a database or third-party APIs

 - *Messaging* - ensure the communication between Cloud Foundry components

- *Metrics and logging* - aggregates the system and application metrics and logs

#### Cloud Foundry sections of note

- Marketplace and Services - research the service catalog and explore any integrated services
- Organizations - used to manage multi-tenancy, quotas, and access to applications
- Routes - list all available endpoints used to access deployed applications
- Build Packs - explore available buildpacks packages used to build an application
- Security groups - configure the endpoints that the application can communicate with

### Cloud Foundry Vs Kubernetes


It is pivotal to understand the application functionalities and available resources. This is especially the case when a microservice-based design is chosen, and solutions suck as IaaS (Infrastructure as a Service), PaaS (Platform as a Service), SaaS (Software as a Service) are available from a multitude of vendors. Choosing the most suitable deployment tooling will lead to the efficient delivery of the product.

Considering that the application code is available, these are the steps to adopt each proposed solution:

- *Kubernetes*
   - create an OCI (Open Container Initiative) compliant image, usually created by using Docker
   - deploy a Kubernetes cluster with a valid ingress controller for the routing of requests
  -  deploy an observability stack, including logs and metrics
  - create the YAML manifests for the application deployment
  - create a CI/CD pipeline to push the Kubernetes resources to the cluster

- *Cloud Foundry*
  - write a manifest file to provide main application deployment parameters
  - deploy Cloud Foundry or use Cloud Foundry PaaS solutions from 3rd part vendors
  - deploy the application to Cloud Foundry (via CLI or UI)
  - Note: Cloud Foundry will create the OCI compliant image by default, and it will provide the routing capacities as well.

Cloud Foundry provides a better developer experience for application deployment, as it offers a greater level of component abstraction (no need to manage the underlying infrastructure). However, a PaaS solution locks-in the customer to a specific vendor. On the other side, Kubernetes offers full control over the container orchestration, providing more flexible management of the application.
