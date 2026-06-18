# Day 50 - Kubernetes Cluster Setup with KIND

## Kubernetes History (In My Own Words)

Kubernetes was originally developed by Google based on their experience running large-scale containerized applications. It was released as an open-source project in 2014 and is now maintained by the Cloud Native Computing Foundation (CNCF). Kubernetes automates the deployment, scaling, networking, and management of containers. Today, it has become the industry standard for container orchestration.

---

## Architecture Diagram

```text
+-----------------------+
|  sample-cluster       |
+-----------------------+
            |
            |
    +----------------+
    | Control Plane  |
    +----------------+
            |
  -----------------------
  |         |          |
  |         |          |
+------+ +------+ +------+
|Worker| |Worker| |Worker|
| Node | | Node | | Node |
+------+ +------+ +------+

Control Plane Components:
- kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager

Worker Components:
- kube-proxy
- kindnet
- Application Pods
```

---

## Tool Chosen: KIND

I chose KIND (Kubernetes IN Docker) because it is lightweight, easy to set up, and runs Kubernetes clusters inside Docker containers. It is ideal for local development, testing, and learning Kubernetes concepts without requiring a virtual machine. KIND also closely resembles a real Kubernetes environment while consuming fewer system resources.

---

## kubectl get nodes

**Screenshot:** *(Insert screenshot here)*

Example Output:

```bash
kubectl get nodes

NAME                           STATUS   ROLES           AGE   VERSION
sample-cluster-control-plane   Ready    control-plane   29m   v1.36.1
sample-cluster-worker          Ready    <none>          29m   v1.36.1
sample-cluster-worker2         Ready    <none>          29m   v1.36.1
sample-cluster-worker3         Ready    <none>          29m   v1.36.1
```

---

## kubectl get pods -n kube-system

**Screenshot:** *(Insert screenshot here)*

Example Output:

```bash
kubectl get pods -n kube-system
```

```text
coredns
etcd
kindnet
kube-apiserver
kube-controller-manager
kube-proxy
kube-scheduler
```

---

## What Each kube-system Pod Does

### CoreDNS

Provides DNS services inside the cluster so pods can communicate using service names instead of IP addresses.

### etcd

The distributed key-value database that stores all Kubernetes cluster state and configuration data.

### kube-apiserver

Acts as the front door of Kubernetes. All kubectl commands and cluster communications go through the API server.

### kube-controller-manager

Runs controllers that continuously monitor cluster state and make changes to achieve the desired state.

### kube-scheduler

Decides which worker node should run a newly created pod based on available resources and scheduling rules.

### kube-proxy

Handles networking on worker nodes and enables communication between services and pods.

### kindnet

The Container Network Interface (CNI) used by KIND to provide pod-to-pod networking across nodes.

### local-path-provisioner

Provides dynamic local storage provisioning for Persistent Volumes in the KIND cluster.

---

## Summary

Successfully created a multi-node Kubernetes cluster using KIND with one control-plane node and three worker nodes. Verified cluster health using kubectl commands, explored Kubernetes system components, and gained hands-on experience with cluster administration and architecture.
