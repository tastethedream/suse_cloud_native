# Lesson 3 

## Kunernetes Resources 

Kubernetes provides a rich collection of resources that are used to deploy, configure, and manage an application. Some of the widely used resources are:

- Pods - the atomic element within a cluster to manage an application

- Deployments & ReplicaSets - oversees a set of pods for the same application

- Services & Ingress - ensures connectivity and reachability to pods

- Configmaps & Secrets - pass configuration to pods
- Namespaces - provides a logical separation between multiple applications and their resources

- Custom Resource Definition (CRD) - extends Kubernetes API to support custom resources

### Application Deployment

A *pod* is the anatomic element within a cluster that provides the execution environment for an application. Pods are the smallest manageable units in a Kubernetes cluster. Every pod has a container within it, that executes an application from a Docker image (or any OCI-compliant image). There are use cases where 2-3 containers run within the same pod, however, it is highly recommended to keep the 1:1 ratio between your pods and containers.

All the pods are placed on the cluster nodes. A note can host multiple pods for different applications.

### Deployments and ReplicaSets

To deploy an application to a Kubernetes cluster, a *Deployment* resource is necessary. A Deployment contains the specifications that describe the desired state of the application. Also, the Deployment resource manages pods by using a *ReplicaSet*. A ReplicaSet resource ensures that the desired amount of replicas for an application are up and running at all times.

To create a deployment, use the `kubectl create deployment` command, with the following syntax:

`kubectl create deploy NAME --image=image [FLAGS] -- [COMMAND] [args]`

Some of the widley used flags are :

- -r, --replicas - set the number of replicas
- -n, --namespace - set the namespace to run
- --port - expose the container port

For example, to create a Deployment for the Go hello-world application, the following command can be used:

`kubectl create deploy go-helloworld --image=pixelpotato/go-helloworld:v1.0.0 -n test`

It is possible to create headless pods or pods that are not managed through a ReplicaSet and Deployment. Though it is not recommended to create standalone pods, these are useful when creating a testing pod.

To create a headless pod, the `kubectl run` command is handy, with the following syntax:

```
# create a headless pod
# NAME - required; set the name of the pod
# IMAGE - required;  specify the Docker image to be executed
# FLAGS - optional; provide extra configuration parameters for the resource
# COMMAND and args - optional; instruct the container to run specific commands when it starts
kubectl run NAME --image=image [FLAGS] -- [COMMAND] [args...]

# Some of the widely used FLAGS are:
--restart - set the restart policy. Options [Always, OnFailure, Never]
--dry-run - dry run the command. Options [none, client, server]
-it - open an interactive shell to the container
```

For example, to create a busybox pod, the following command can be used:

`kubectl run -it busybox-test --image=busybox --restart=Never`

### Rolling Out Strategy

The Deployment resource comes with a very powerful rolling out strategy, which ensures that no downtime is encountered when a new version of the application is released. Currently, there are 2 rolling out strategies:

- RollingUpdate - updates the pods in a rolling out fashion (e.g. 1-by-1)
- Recreate - kills all existing pods before new ones are created


## Application Reachability

Within a cluster, every pod is allocated 1 unique IP which ensures connectivity and reachability to the application inside the pod. This IP is only routable inside the cluster, meaning that external users and services will not be able to connect to the application.

For example, we can connect a workload within the cluster to access a pod directly via its IP. However, if the pod dies, all future requests will fail, as these are routes to an application that is not running. The remediation step is to configure the workload to communicate with a different pod IP. This is a highly manual process, which brings complexity to the accessibility of an application. To automate the reachability to pods, a Service resource is necessary.

### Services

A Service resource provides an abstraction layer over a collection of pods running an application. A Service is allocated a cluster IP, that can be used to transfer the traffic to any available pods for an application.

As such, as shown in the above image, instead of accessing each pod independently, the workload (1) should access the service IP (2), which routes the requests to available pods (3).

There are 3 widely used Service types:

- *ClusterIP* - exposes the service using an internal cluster IP. If no service type is specified, a ClusterIP service is created by default.

- *NodePort* - expose the service using a port exposed on all nodes in the cluster.

- *LoadBalancer* - exposes the service through a load balancer from a public cloud provider such as AWS, Azure, or GCP. This will allow the external traffic to reach the services within the cluster securely.

To create a service for an existing deployment, use the kubectl expose deployment command, with the following syntax:

```
# expose a Deployment through a Service resource
# NAME - required; set the name of the deployment to be exposed
# --port - required; specify the port that the service should serve on
# --target-port - optional; specify the port on the container that the service should direct traffic to
# FLAGS - optional; provide extra configuration parameters for the service
kubectl expose deploy NAME --port=port [--target-port=port] [FLAGS]

# Some of the widely used FLAGS are:
--protocol - set the network protocol. Options [TCP|UDP|SCTP]
--type - set the type of service. Options [ClusterIP, NodePort, LoadBalancer]
```

For example, to expose the Go hello-world application through a service, the following command can be used:

```
# expose the `go-helloworld` deployment on port 8111
# note: the application is serving requests on port 6112
kubectl expose deploy go-helloworld --port=8111 --target-port=6112
```

### Ingress

To enable the external user to access services within the cluster an Ingress resource is necessary. An Ingress exposes HTTP and HTTPS routes to services within the cluster, using a load balancer provisioned by a cloud provider. Additionally, an Ingress resource has a set of rules that are used to map HTTP(S) endpoints to services running in the cluster. To keep the Ingress rules and load balancer up-to-date an Ingress Controller is introduced.

For example, as shown in the image above, the customers will access the go-helloworld.com/hi HTTP route (1), which is managed by an Ingress (2). The Ingress Controller (3) examines the configured routes and directs the traffic to a LoadBalancer (4). And finally, the LoadBalancer directs the requests to the pods using a dedicated port (5).

## Application Configuration and Context

In the implementation phase, a good development practice is to separate the configuration from the source code. This increased the portability of an application as it can cover multiple customer use cases. Kubernetes has 2 resources to pass data to an application: Configmaps and Secrets.

### ConfigMaps

ConfigMaps are objects that store non-confidential data in key-value pairs. A Configmap can be consumed by a pod as an environmental variable, configuration files through a volume mount, or as command-line arguments to the container.

To create a Configmap use the `kubectl create configmap` command, with the following syntax:

```
# create a Configmap
# NAME - required; set the name of the configmap resource
# FLAGS - optional; define  extra configuration parameters for the configmap
kubectl create configmap NAME [FLAGS]

# Some of the widely used FLAGS are:
--from-file - set path to file with key-value pairs
--from-literal - set key-value pair from command-line
```

For example, to create a Configmap to store the background color for a front-end application, the following command can be used:

```
# create a Configmap to store the color value
kubectl create configmap test-cm --from-literal=color=yellow
```

### Secrets

Secrets are used to store and distribute sensitive data to the pods, such as passwords or tokens. Pods can consume secrets as environment variables or as files from the volume mounts to the pod. It is noteworthy, that Kubernetes will encode the secret values using base64.

To create a Secret use the `kubectl create secret generic` command, with the following syntax:

```
# create a Secret
# NAME - required; set the name of the secret resource
# FLAGS - optional; define  extra configuration parameters for the secret
kubectl create secret generic NAME [FLAGS]

# Some of the widely used FLAGS are:
--from-file - set path to file with the sensitive key-value pairs
--from-literal - set key-value pair from command-line
```

For example, to create a Secret to store the secret background color for a front-end application, the following command can be used:

```
# create a Secret to store the secret color value
kubectl create secret generic test-secret --from-literal=color=blue
```

### Namespaces

A Kubernetes cluster is used to host hundreds of applications, and it is required to have separate execution environments across teams and business verticals. This functionality is provisioned by the *Namespace* resources. A Namespace provides a logical separation between multiple applications and associated resources. In a nutshell, it provides the *application context*, defining the environment for a group of Kubernetes resources that relate to a project, such as the amount of CPU, memory, and access. For example, a project-green namespace includes any resources used to deploy the `Green Project`. These resources construct the application context and can be managed collectively to ensure a successful deployment of the project.

Each team or business vertical is allocated a separate Namespace, with the desired amount of CPU, memory, and access. This ensures that the application is managed by the owner team and has enough resources to execute successfully. This also eliminates the "noisy neighbor" use case, where a team can consume all the available resources in the cluster if no Namespace boundaries are set.

To create a Namespace we can use the `kubectl create namespace` command, with the following syntax:

```
# create a Namespace
# NAME - required; set the name of the Namespace
kubectl create ns NAME
```

For example, to create a `test-udacity` Namespace, the following command can be used:

```
# create a `test-udacity` Namespace
kubectl create ns test-udacity

# get all the pods in the `test-udacity` Namespace
kubectl get po -n test-udacity
```


