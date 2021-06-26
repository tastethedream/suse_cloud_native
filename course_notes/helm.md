# Lesson 5 : Helm Walkthrough

- [Python hello-world](https://github.com/udacity/nd064_course_1/tree/main/solutions/helm/python-helloworld) Helm chart that is used to parameterized the Namespace and Deployment resources. Note how the `values.yaml` and `values-prod.yaml` files are used to overwrite name of the Namespace.

- The Application CRD to deploy the Helm chart referencing the default input values file, can be found in `argocd-helm-python.yaml` manifest

- The Application CRD used to deploy the Helm chart referencing the `values-prod.yaml` file, can be found in `argocd-helm-python-prod.yaml` manifest

Helm provides a powerful mechanism to inject values to templated YAML manifests.

The full Helm chart for `nginx-deployment` can found in the course repository.

The Helm chart is defined in the `Chart.yaml` file, which contains the API version, name and version of the chart:

```apiVersion: v1
name: nginx-deployment
description: Install Nginx deployment manifests 
keywords:
- nginx 
version: 1.0.0
maintainers:
- name: kgamanji 
```

An example of the `values.yaml` file can be found below:

```
namespace:
  name: demo

service:
  port: 8111
  type: ClusterIP

image:
  repository: nginx 
  tag: alpine
  pullPolicy: IfNotPresent

replicaCount: 3

resources:
  requests:
    cpu: 50m
    memory: 256Mi

configmap:
  data: "version: alpine"
```

The above configuration represents the default parameters of application deployment if it is not overwritten by a different values file.

Below is an example of the `values-prod.yaml` file, which will override the default parameters:

```
namespace:
  name: prod

service:
  port: 80
  type: ClusterIP

image:
  repository: nginx
  tag: 1.17.0
  pullPolicy: IfNotPresent

replicaCount: 2

resources:
  requests:
    cpu: 70m
    memory: 256Mi

configmap:
  data: "version: 1.17.0"
```

And finally, here is ArgoCD application CRD for the `nginx-prod` deployment:

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx-prod
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    helm:
      valueFiles:
      - values-prod.yaml
    path: helm
    repoURL: https://github.com/kgamanji/argocd-demo
    targetRevision: HEAD
```

### Deploy Helm Chart

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx-prod
  namespace: argocd
spec:
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  project: default
  source:
    helm:
      valueFiles:
      - values-prod.yaml
    path: helm
    repoURL: https://github.com/kgamanji/argocd-demo
    targetRevision: HEAD
```
