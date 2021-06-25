# Lesson 5 : CI/CD with Cloud Native Tooling

## Application Releases 

Up to this stage, we have practiced the packaging of an application using Docker and its deployment to a Kubernetes cluster using kubectl commands. As well, we have explored the simplified developer experience of application release with Cloud Foundry. However, in both cases, a user has to manually trigger and complete all the operations. This is not sustainable if tens and hundreds of releases are performed within a day. Automation of the release process is fundamental!

In the case of a PaaS offering, the release of a new feature is managed by the provider. For example, Cloud Foundry monitors the repository with the source code, and when a new commit is identified, the user can easily deploy the latest changes with a click of a button. On the other side, releasing an application to a Kubernetes cluster consists of a series of manually typed `docker` and `kubectl` commands. At this stage, this approach has no automation integrated.

In this lesson, we will not cover how a PaaS automates the release process, since this is already solutionized by the 3rd party providers. Instead, we will focus on building a delivery pipeline to automate the deployment to Kubernetes using cloud-native tooling.

## Contunuous Application Deployment

Every company has the same goal: to deliver value to customers and maintain customer satisfaction. To achieve this, an organization needs to be fast in integrating customer feedback and release new features.

It is possible to manually deploy every release for a small product. However, this is not viable for a product that has thousands of microservices developed by hundreds of engineers. A delivery pipeline is essential for continuous and automated deployment of new functionalities.

A delivery pipeline includes stages that can test, validate, package, and push new features to a production environment. It is common practice for the main branch commits to proceed through all stages of the pipeline to reach the end-users. Overall, a delivery pipeline is triggered when a new commit is available. The new changes should traverse the following stages:

- *Build* - compile the application source code and its dependencies. If this stage fails the developer should address it immediately as there might be missing dependencies or errors in the code.

- *Test* - run a suite of tests, such as unit testing, integration, UI, smoke, or security tests. These tests aim to validate the behavior of the code. If this stage fails, then developers must correct it to prevent dysfunctional code from reaching the end-users.

- *Package* - create an executable that contains the latest code and its dependencies. This is a runnable instance of the application that can be deployed to end-users.

- *Deploy* - push the packaged application to one or more environments, such as sandbox, staging, and production. Usually, the sandbox and staging deployments are automatic, and the production deployment requires engineering validation and triggering.

It is common practice to push an application through multiple environments before it reaches the end-users. Usually, these are categorized as follows:

- *Sandbox* - development environment, where new changes can be tested with minimal risk.

- *Staging* - an environment identical to production, and where a release can be simulated without affecting the end-user experience.

- *Production* - customer-facing environment and any changes in this environment will affect the customer experience.

Overall, a delivery pipeline consists of two phases:

- *Continuous Integration* (or CI) includes the *build, test, and package* stages.

- *Continuous Delivery* (or CD) handles the *deploy* stage.

### Advantages of a CI/CD pipeline

- *Frequent releases* - automation enables engineers to ship new code as soon as it's available and improves responsiveness to customer feedback.

- *Less risk* - automation of releases eliminates the need for manual intervention and configuration.

- *Developer productivity* - a structured release process allows every product to be released independently of other components


