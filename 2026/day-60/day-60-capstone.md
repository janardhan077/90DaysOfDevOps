# Day 60 – Kubernetes Capstone Project

# Kubernetes Capstone – WordPress + MySQL

## Overview

This capstone project combines the major Kubernetes concepts learned over the past ten days into a single real-world application deployment.

The application consists of:

- WordPress (Frontend)
- MySQL (Database)

The deployment demonstrates:

- Pods
- Deployments
- Services
- ConfigMaps
- Secrets
- Persistent Volumes (PV)
- Persistent Volume Claims (PVC)
- StatefulSets
- Resource Requests & Limits
- Health Probes
- Horizontal Pod Autoscaler (HPA)
- Helm (optional deployment method)

The objective is to build a resilient, scalable, and persistent application that can recover from failures while maintaining application data.

---

# Architecture of the Deployment

The deployment is composed of several Kubernetes resources that work together.

```
                    User
                      │
                      ▼
             WordPress Service
                      │
                      ▼
          WordPress Deployment
                      │
      ┌───────────────┴───────────────┐
      │                               │
      ▼                               ▼
 ConfigMap                      Secret
(App Configuration)      (Database Credentials)
      │                               │
      └───────────────┬───────────────┘
                      │
                      ▼
               MySQL Service
                      │
                      ▼
            MySQL StatefulSet
                      │
                      ▼
        Persistent Volume Claim (PVC)
                      │
                      ▼
          Persistent Volume (PV)
                      │
                      ▼
              Physical Storage
```

### Resource Relationships

- Users access the application through the **WordPress Service**.
- The **WordPress Deployment** manages frontend Pods.
- A **ConfigMap** provides non-sensitive configuration values.
- A **Secret** stores database credentials securely.
- WordPress connects to MySQL using the **MySQL Service**.
- MySQL runs as a **StatefulSet** to ensure stable identities and persistent storage.
- Each MySQL Pod receives its own **Persistent Volume Claim (PVC)**.
- The PVC is bound to a **Persistent Volume (PV)** where database data is stored.

---

# Self-Healing Test Results

Kubernetes continuously monitors application health and automatically recovers from failures.

### Test Performed

Delete the running WordPress Pod:

```bash
kubectl delete pod <wordpress-pod>
```

### Result

- Kubernetes immediately created a replacement Pod.
- The new Pod became **Running**.
- The application became available again without manual intervention.

---

Delete the MySQL Pod:

```bash
kubectl delete pod mysql-0
```

### Result

- StatefulSet recreated the Pod automatically.
- The Pod retained the same name (`mysql-0`).
- The database remained accessible after recovery.

---

### Observation

Self-healing worked successfully because:

- Deployment recreated stateless Pods.
- StatefulSet recreated stateful Pods while preserving identity and storage.

---

# Persistence Test Results

To verify persistent storage, data was written to the MySQL database.

### Test Steps

1. Create a sample database or table.
2. Delete the MySQL Pod.
3. Wait for Kubernetes to recreate the Pod.
4. Reconnect to MySQL.
5. Verify that the data still exists.

### Result

- Database data remained intact.
- Persistent Volume successfully retained storage.
- PVC was automatically reattached to the recreated Pod.

### Observation

Persistent storage ensured that application data survived Pod recreation.

---

# Kubernetes Concepts Learned

| Day | Concept |
|-----|---------|
| Day 51 | Pods |
| Day 52 | Deployments & Services |
| Day 53 | Services & Networking |
| Day 54 | ConfigMaps & Secrets |
| Day 55 | Persistent Volumes (PV) & Persistent Volume Claims (PVC) |
| Day 56 | StatefulSets |
| Day 57 | Resource Requests, Limits & Health Probes |
| Day 58 | Horizontal Pod Autoscaler (HPA) |
| Day 59 | Helm |
| Day 60 | Kubernetes Capstone Project |

---

# Reflection

## What Was the Hardest?

The most challenging parts were understanding how different Kubernetes resources work together and troubleshooting issues during deployment.

Some examples included:

- Debugging Pods stuck in `CrashLoopBackOff`
- Fixing configuration mistakes
- Understanding StatefulSets and persistent storage
- Learning how Services and DNS enable communication between Pods

These challenges helped build confidence in diagnosing Kubernetes workloads.

---

## What Clicked?

Several concepts became much clearer through hands-on practice:

- Deployments automatically maintain the desired number of Pods.
- Services provide stable networking between applications.
- ConfigMaps and Secrets separate configuration from application code.
- StatefulSets provide stable identities and persistent storage.
- PVCs allow applications to keep data even after Pod recreation.
- Health probes enable Kubernetes to detect and recover from unhealthy containers.
- HPA automatically scales applications based on resource usage.
- Helm simplifies application deployment and management using reusable charts.

---

## What Would Be Added for Production?

A production-ready deployment would typically include:

- Ingress for external access
- TLS certificates (HTTPS)
- Network Policies
- Resource Quotas and LimitRanges
- Monitoring with Prometheus and Grafana
- Centralized logging with EFK or Loki
- Backup and disaster recovery strategy
- Pod Disruption Budgets (PDBs)
- Multiple replicas for high availability
- CI/CD pipelines for automated deployments
- Image vulnerability scanning
- Role-Based Access Control (RBAC)
- External Secrets management
- Multi-node Kubernetes cluster

---

# Key Takeaways

- Kubernetes enables applications to be resilient, scalable, and self-healing.
- Deployments manage stateless workloads, while StatefulSets manage stateful workloads.
- ConfigMaps and Secrets separate configuration from application code.
- Persistent Volumes ensure data survives Pod recreation.
- Resource requests, limits, and health probes improve workload reliability.
- Horizontal Pod Autoscaler enables automatic scaling based on demand.
- Helm simplifies deployment, upgrades, and rollback of complex applications.
- Combining these concepts results in a production-ready architecture for modern cloud-native applications.
