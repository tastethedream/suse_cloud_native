# Suse Cloud Native Scholarship: Lesson 3

## Deploy your first kubernetes cluster

### Cluster Creation

Provisioning a Kubernetes cluster is known as the bootstrapping process. When creating a cluster, it is essential to ensure that each node had the necessary components installed. It is possible to manually provision a cluster, however, this implied the distribution and execution of each component independently (e.g. kube-apiserver, kube-scheduler, kubelet, etc.). This is a highly tedious task that has a higher risk of misconfiguration.

As a result, multiple tools emerged to handle the bootstrapping of a cluster automatically. For example:

- Production-grade clusters
  - kubeadm
  - Kubespray
  - Kops
  - K3s
- Developmnet-grade clusters
  - kind
  - minikube
  - k3d

A good introduction to k3d, written by k3d's creator Thorsten Klein, can be found [here] (https://www.suse.com/c/introduction-k3d-run-k3s-docker-src/)

