# Day 56 – Kubernetes StatefulSets

# StatefulSets in Kubernetes

## What is a StatefulSet?

A **StatefulSet** is a Kubernetes workload resource used to manage **stateful applications**. Unlike Deployments, StatefulSets provide each Pod with:

- A **stable, unique identity**
- A **persistent storage volume**
- A **predictable network name**
- Ordered deployment, scaling, and termination

StatefulSets are designed for applications where each Pod must maintain its own identity and data.

---

# Why Use StatefulSets?

Most applications fall into two categories:

### Stateless Applications

These applications do not store important data inside the container.

Examples:
- Nginx
- Apache
- React frontend
- Node.js API
- Spring Boot REST API

For these, **Deployments** are the preferred choice.

---

### Stateful Applications

These applications store data that must survive Pod restarts.

Examples:
- MySQL
- PostgreSQL
- MongoDB
- Cassandra
- Redis (persistent mode)
- Kafka
- Elasticsearch
- ZooKeeper

For these, **StatefulSets** are the recommended choice.

---

# StatefulSet vs Deployment

| Feature | Deployment | StatefulSet |
|----------|------------|-------------|
| Pod Identity | Random | Stable and unique |
| Pod Names | Change after recreation | Remain consistent |
| Network Identity | Temporary | Stable DNS name |
| Storage | Shared or ephemeral | Dedicated persistent volume per Pod |
| Scaling | Any order | Ordered |
| Updates | Parallel by default | Ordered rolling updates |
| Pod Deletion | Any order | Reverse order |
| Best For | Stateless apps | Stateful apps |

---

# When to Use StatefulSets vs Deployments

## Use Deployments When

- Running stateless applications
- Pods are interchangeable
- No persistent identity is needed
- Data is stored externally

Examples:
- Web servers
- APIs
- Frontend applications

---

## Use StatefulSets When

- Each Pod requires its own storage
- Stable Pod names are required
- Applications depend on predictable startup order
- Cluster members must discover each other

Examples:
- Databases
- Distributed systems
- Message brokers

---

# Stable Pod Identity

Deployment Pods receive changing names.

Example:

```
nginx-7d4c65d5d7-abc12
nginx-7d4c65d5d7-x9k2q
```

After recreation:

```
nginx-8b765dd67d-pm82c
```

The name changes.

---

StatefulSet Pods always keep their names.

Example:

```
mysql-0
mysql-1
mysql-2
```

Even after restarting:

```
mysql-0
mysql-1
mysql-2
```

The identity remains the same.

---

# Headless Service

A StatefulSet normally uses a **Headless Service**.

Instead of load balancing traffic, it provides **direct DNS records** for each Pod.

Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
```

Notice:

```
clusterIP: None
```

This makes the Service headless.

---

# Stable DNS

With a Headless Service named **mysql** and a StatefulSet named **mysql**, each Pod gets a predictable DNS name.

Example:

```
mysql-0.mysql.default.svc.cluster.local

mysql-1.mysql.default.svc.cluster.local

mysql-2.mysql.default.svc.cluster.local
```

Applications can reliably communicate using these DNS names, even after Pods restart.

---

# volumeClaimTemplates

Each StatefulSet Pod usually requires its own persistent storage.

Instead of creating multiple PVCs manually, StatefulSets use **volumeClaimTemplates**.

Example:

```yaml
volumeClaimTemplates:
- metadata:
    name: mysql-data
  spec:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 10Gi
```

Kubernetes automatically creates one PVC per Pod.

Example:

```
mysql-data-mysql-0

mysql-data-mysql-1

mysql-data-mysql-2
```

Each Pod receives its own dedicated Persistent Volume.

---

# Ordered Deployment

Pods are created sequentially.

```
mysql-0

↓

mysql-1

↓

mysql-2
```

The next Pod is created only after the previous one becomes Ready.

---

# Ordered Deletion

Pods are removed in reverse order.

```
mysql-2

↓

mysql-1

↓

mysql-0
```

This helps maintain application stability during scale-down operations.

---

# Advantages of StatefulSets

- Stable Pod names
- Predictable DNS
- Persistent storage
- Ordered startup and shutdown
- Automatic PVC creation
- Ideal for clustered applications

---

# Screenshots to Include

## 1. StatefulSet Pods

Capture the output of:

```bash
kubectl get pods
```

Example:

```
NAME      READY   STATUS    AGE
mysql-0   1/1     Running   10m
mysql-1   1/1     Running   9m
mysql-2   1/1     Running   8m
```

---

## 2. Persistent Volume Claims (PVCs)

Capture the output of:

```bash
kubectl get pvc
```

Example:

```
NAME                 STATUS   VOLUME      CAPACITY
mysql-data-mysql-0   Bound    pvc-xxxx    10Gi
mysql-data-mysql-1   Bound    pvc-yyyy    10Gi
mysql-data-mysql-2   Bound    pvc-zzzz    10Gi
```

---

## 3. DNS Resolution

Run the following from a test Pod:

```bash
nslookup mysql-0.mysql
nslookup mysql-1.mysql
nslookup mysql-2.mysql
```

or

```bash
kubectl exec -it <test-pod> -- nslookup mysql-0.mysql
```

Capture the output showing that each Pod resolves to its own IP address.

---

# Summary

- **StatefulSets** manage applications that require stable identities and persistent storage.
- Use **Deployments** for stateless workloads and **StatefulSets** for databases, message brokers, and clustered applications.
- A **Headless Service (`clusterIP: None`)** provides direct DNS records for each Pod instead of load balancing.
- Each StatefulSet Pod receives a stable DNS name (for example, `mysql-0.mysql.default.svc.cluster.local`).
- **volumeClaimTemplates** automatically create one PersistentVolumeClaim for each Pod.
- StatefulSets create Pods in order and delete them in reverse order, ensuring predictable behavior for stateful applications.
