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



