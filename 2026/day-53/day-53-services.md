# Day 53 - Kubernetes Services

## Objective

Learned how Kubernetes Services provide stable networking for applications, enable service discovery, and expose applications inside or outside the cluster.

---

# Why do Kubernetes Services exist?

Pods in Kubernetes are **ephemeral**, meaning they can be created, deleted, or recreated at any time. Whenever a Pod is recreated, it receives a **new IP address**.

If applications communicate directly with Pod IPs, communication will fail whenever the Pod IP changes.

A Kubernetes Service solves this problem by providing:

- A stable virtual IP (ClusterIP)
- A permanent DNS name
- Load balancing across multiple Pods
- A single entry point to access an application

### Relationship between Pods, Deployments, and Services

- **Deployment**
  - Creates and manages Pods.
  - Ensures the desired number of replicas are always running.

- **Pods**
  - Run the application containers.
  - Have temporary IP addresses.

- **Service**
  - Selects Pods using labels.
  - Provides a stable IP and DNS name.
  - Routes traffic to healthy Pods.

```
Client
   │
   ▼
Service
   │
   ├────────► Pod 1
   ├────────► Pod 2
   └────────► Pod 3
```

---

# Service Manifest 1 - ClusterIP

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

### Explanation

- Creates an internal Service.
- Accessible only inside the Kubernetes cluster.
- Selects Pods with the label:

```yaml
app: web-app
```

- Exposes port **80**.
- Forwards traffic to container port **80**.

Use case:

- Backend APIs
- Databases
- Internal microservices

---

# Service Manifest 2 - NodePort

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

### Explanation

- Exposes the application on every Kubernetes node.
- External users can access the application using:

```
<Node-IP>:30080
```

Traffic flow:

```
Browser
    │
NodeIP:30080
    │
NodePort Service
    │
Pods
```

Use case:

- Development
- Testing
- Local Kubernetes clusters

---

# Service Manifest 3 - LoadBalancer

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

### Explanation

- Requests an external cloud load balancer.
- Available on managed Kubernetes platforms like:
  - AWS EKS
  - Azure AKS
  - Google GKE

Traffic flow:

```
Internet
    │
Load Balancer
    │
Service
    │
Pods
```

Use case:

- Public web applications
- APIs
- Production workloads

---

# Difference Between Service Types

| Feature | ClusterIP | NodePort | LoadBalancer |
|----------|-----------|----------|--------------|
| Internal Access | ✅ Yes | ✅ Yes | ✅ Yes |
| External Access | ❌ No | ✅ Yes | ✅ Yes |
| Stable Cluster IP | ✅ Yes | ✅ Yes | ✅ Yes |
| Node Port | ❌ No | ✅ Yes | Usually Yes |
| Cloud Load Balancer | ❌ No | ❌ No | ✅ Yes |
| Best For | Internal apps | Development & testing | Production |

---

# Kubernetes DNS Service Discovery

Every Service automatically receives a DNS name.

Example:

```
Service Name:
web-app-cluster
```

Namespace:

```
default
```

Full DNS name:

```
web-app-cluster.default.svc.cluster.local
```

Applications can communicate using:

```
http://web-app-cluster
```

instead of remembering IP addresses.

Example:

```
Frontend Pod
       │
       ▼
http://web-app-cluster
       │
       ▼
Backend Pods
```

DNS is provided by **CoreDNS**, which runs inside the Kubernetes cluster.

---

# What are Endpoints?

A Service itself does not contain application code.

Instead, it maintains a list of Pod IPs called **Endpoints**.

Example:

```
Service

web-app-cluster

↓

Endpoints

10.244.1.3
10.244.2.3
10.244.4.3
```

Whenever a request reaches the Service, Kubernetes forwards it to one of these endpoint Pods.

To inspect Endpoints:

```bash
kubectl get endpoints
```

Detailed view:

```bash
kubectl describe endpoints web-app-cluster
```

Example output:

```
NAME              ENDPOINTS
web-app-cluster   10.244.1.3:80,10.244.2.3:80,10.244.4.3:80
```

---

# Verification

### List Services

```bash
kubectl get svc
```

Example:

```
NAME                  TYPE        CLUSTER-IP      PORT(S)
web-app-cluster       ClusterIP   10.96.125.2     80/TCP
web-app-nodeport      NodePort    10.96.190.168   80:30080/TCP
```

---

### DNS Test

```bash
nslookup web-app-cluster
```

Output:

```
Name:
web-app-cluster.default.svc.cluster.local

Address:
10.96.125.2
```

The resolved IP matches the Service's ClusterIP.

---

### Check Endpoints

```bash
kubectl get endpoints
```

Example:

```
NAME              ENDPOINTS
web-app-cluster   10.244.1.3:80,10.244.2.3:80,10.244.4.3:80
```

---

### Test Service

```bash
curl http://web-app-cluster
```

or

```bash
curl <Node-IP>:30080
```

The request is forwarded to one of the running Pods.

---

# Screenshots

Include the following screenshots:

1. `kubectl get svc`
2. `kubectl get endpoints`
3. `kubectl get pods -o wide`
4. `nslookup web-app-cluster`
5. `curl` test output showing the application response

---

# Key Takeaways

- Services provide stable networking for Pods.
- Pods can change IP addresses, but Services maintain a constant IP and DNS name.
- Services use labels and selectors to find Pods.
- ClusterIP is used for internal communication.
- NodePort exposes applications through a node's IP and port.
- LoadBalancer exposes applications using a cloud load balancer.
- CoreDNS enables service discovery using DNS names.
- Endpoints contain the list of Pod IPs that receive traffic.
