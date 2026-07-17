# Day 53 – Kubernetes Services

## What I Learned

Today I learned about **Kubernetes Services** and why they are essential for communication between applications running inside a Kubernetes cluster.

One important thing I understood is that **Pods are temporary (ephemeral)**. If a Pod crashes or is recreated, it gets a new IP address. This makes it unreliable to communicate directly with Pods.

A **Service** solves this problem by providing a **stable IP address and DNS name**. Instead of connecting to individual Pods, applications connect to the Service, and Kubernetes automatically forwards the traffic to the correct Pods.

---

## Relationship Between Deployments, Pods, and Services

- A **Deployment** manages the lifecycle of Pods and ensures the desired number of replicas are running.
- **Pods** run the application containers but have temporary IP addresses.
- A **Service** sits in front of the Pods, provides a permanent endpoint, and distributes incoming requests among the available Pods.

```
Client
   |
   v
Service
   |
   +----> Pod 1
   +----> Pod 2
   +----> Pod 3
```

---

# Service Manifest 1 – ClusterIP

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-cluster
spec:
  type: ClusterIP
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
```

### What it does

This creates an internal Service that is only accessible from within the Kubernetes cluster.

The selector looks for Pods with the label:

```yaml
app: web-app
```

Whenever another Pod accesses this Service, Kubernetes forwards the request to one of the matching Pods.

**Best use cases:**

- Backend services
- Internal APIs
- Databases

---

# Service Manifest 2 – NodePort

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-nodeport
spec:
  type: NodePort
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080
```

### What it does

NodePort exposes the application on every Kubernetes node using a specific port.

The application can be accessed using:

```
<Node-IP>:30080
```

This is useful for testing applications on a local Kubernetes cluster without using a cloud load balancer.

---

# Service Manifest 3 – LoadBalancer

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-loadbalancer
spec:
  type: LoadBalancer
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 80
```

### What it does

This Service type is mainly used in cloud environments like AWS, Azure, or Google Cloud.

Kubernetes requests an external load balancer from the cloud provider and forwards internet traffic to the application.

This is commonly used for production applications.

---

# Difference Between ClusterIP, NodePort, and LoadBalancer

| Service Type | Description | Best Used For |
|--------------|-------------|---------------|
| **ClusterIP** | Accessible only inside the cluster | Internal communication |
| **NodePort** | Exposes the application through a node's IP and a fixed port | Local testing and development |
| **LoadBalancer** | Creates an external cloud load balancer | Production applications |

---

# Kubernetes DNS

Kubernetes automatically creates a DNS name for every Service.

For example, if the Service name is:

```
web-app-cluster
```

and it is in the default namespace, the full DNS name becomes:

```
web-app-cluster.default.svc.cluster.local
```

Instead of remembering IP addresses, applications can simply communicate using:

```
http://web-app-cluster
```

CoreDNS is responsible for resolving these names to the correct Service IP.

During testing, I used:

```bash
nslookup web-app-cluster
```

The returned IP matched the Service's **ClusterIP**, confirming that DNS resolution was working correctly.

---

# What are Endpoints?

A Service doesn't directly know where the application is running.

Instead, Kubernetes creates **Endpoints**, which are simply the IP addresses of all Pods selected by the Service.

For example:

```
Service
    |
    +----> 10.244.1.3
    +----> 10.244.2.3
    +----> 10.244.4.3
```

Whenever a request reaches the Service, Kubernetes forwards it to one of these endpoint Pods.

I inspected the Endpoints using:

```bash
kubectl get endpoints
```

and for more details:

```bash
kubectl describe endpoints web-app-cluster
```

---

# Verification

### List Services

```bash
kubectl get svc
```

### Check Endpoints

```bash
kubectl get endpoints
```

### Test DNS

```bash
nslookup web-app-cluster
```

The DNS name resolved to the same IP shown as the Service's ClusterIP.

### Test the Service

For an internal Service:

```bash
curl http://web-app-cluster
```

For a NodePort Service:

```bash
curl <Node-IP>:30080
```

---

# Screenshots

I have included screenshots of:

- `kubectl get svc`
- `kubectl get endpoints`
- `kubectl get pods -o wide`
- `nslookup web-app-cluster`
- Service testing using `curl`

---

# Key Takeaways

- Pods are temporary and their IP addresses can change.
- Services provide a stable way to communicate with Pods.
- Services use labels and selectors to identify the correct Pods.
- ClusterIP is used for internal communication.
- NodePort allows external access using a node's IP and port.
- LoadBalancer exposes applications through a cloud provider.
- CoreDNS makes Service discovery simple using DNS names.
- Endpoints contain the list of Pod IPs that receive traffic from a Service.
