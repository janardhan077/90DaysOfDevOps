# Day 58 – Kubernetes Metrics Server & Horizontal Pod Autoscaler (HPA)

## Objective

Learn how the Metrics Server provides resource metrics to Kubernetes and how the Horizontal Pod Autoscaler (HPA) uses those metrics to automatically scale applications.

---

# What is the Metrics Server?

The Metrics Server is a Kubernetes add-on that collects CPU and memory usage from every node's Kubelet and exposes those metrics through the Kubernetes Metrics API.

It is designed for autoscaling and monitoring resource usage.

Without the Metrics Server:

- `kubectl top nodes` ❌
- `kubectl top pods` ❌
- Horizontal Pod Autoscaler (HPA) ❌

With the Metrics Server:

- `kubectl top nodes` ✅
- `kubectl top pods` ✅
- Horizontal Pod Autoscaler (HPA) ✅

---

# Why does HPA need the Metrics Server?

The Horizontal Pod Autoscaler makes scaling decisions based on resource metrics such as CPU and memory.

The Metrics Server continuously collects these metrics from every node and makes them available to Kubernetes.

Flow:

```
Users
   │
   ▼
Application Pods
   ▲
   │
Metrics Server
   ▲
   │
Kubelet
   ▲
   │
Node
```

Without the Metrics Server, HPA has no CPU or memory data, so it cannot determine when to increase or decrease replicas.

---

# How HPA Calculates Desired Replicas

HPA compares the current average CPU utilization with the target utilization.

Formula:

```
Desired Replicas =
Current Replicas × (Current CPU Utilization ÷ Target CPU Utilization)
```

Example:

Current replicas = 3

Target CPU = 50%

Current CPU = 90%

```
Desired Replicas = 3 × (90 ÷ 50)

Desired Replicas = 5.4

Rounded = 6 Pods
```

If CPU usage falls below the target, HPA reduces the number of Pods.

---

# Difference Between autoscaling/v1 and autoscaling/v2

| Feature | autoscaling/v1 | autoscaling/v2 |
|----------|----------------|----------------|
| CPU Metrics | ✅ | ✅ |
| Memory Metrics | ❌ | ✅ |
| Custom Metrics | ❌ | ✅ |
| External Metrics | ❌ | ✅ |
| Scaling Behavior | ❌ | ✅ |
| Scale Policies | ❌ | ✅ |
| Stabilization Window | ❌ | ✅ |

Use **autoscaling/v1** for simple CPU-based scaling.

Use **autoscaling/v2** when advanced scaling behavior, multiple metrics, or custom metrics are required.

---

# HPA Configuration Used

```yaml
behavior:
  scaleUp:
    stabilizationWindowSeconds: 0
    policies:
    - type: Pods
      value: 1
      periodSeconds: 15

  scaleDown:
    stabilizationWindowSeconds: 300
    policies:
    - type: Pods
      value: 1
      periodSeconds: 15
```

Scale Up:
- No waiting period
- Adds one Pod every 15 seconds

Scale Down:
- Waits 300 seconds before removing Pods
- Removes one Pod every 15 seconds

---

# Commands Used

Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Check Metrics Server

```bash
kubectl get pods -n kube-system
```

View Node Metrics

```bash
kubectl top nodes
```

View Pod Metrics

```bash
kubectl top pods
```

Create HPA

```bash
kubectl apply -f hpa.yaml
```

View HPA

```bash
kubectl get hpa
```

Describe HPA

```bash
kubectl describe hpa sampuu-app-hpa
```

Watch Pod Scaling

```bash
kubectl get pods -w
```

---

# Screenshots

## 1. kubectl top nodes

> Insert screenshot here

---

## 2. kubectl top pods

> Insert screenshot here

---

## 3. kubectl get hpa

> Insert screenshot here

---

## 4. kubectl describe hpa

> Insert screenshot here

---

## 5. Pod Scaling (kubectl get pods -w)

> Insert screenshot here

---

# Key Takeaways

- Metrics Server provides CPU and memory metrics to Kubernetes.
- HPA automatically adjusts the number of Pods based on resource utilization.
- CPU requests must be configured for HPA to calculate utilization correctly.
- autoscaling/v2 supports advanced scaling behavior and multiple metrics.
- Scaling behavior allows fine-grained control over how quickly Pods are added or removed.

---

# Conclusion

Today I learned how Kubernetes automatically scales applications using the Metrics Server and the Horizontal Pod Autoscaler. I explored how HPA calculates the desired number of replicas, configured custom scaling behavior, monitored CPU usage with `kubectl top`, and observed Pods scaling dynamically based on application load.
