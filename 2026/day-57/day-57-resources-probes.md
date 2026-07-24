# Day 57 – Kubernetes Resources & Probes

# Managing Resources and Health Checks in Kubernetes

Kubernetes helps ensure applications run reliably by managing **CPU and memory resources** and continuously checking the health of containers using **probes**.

Two important concepts are:

- **Resource Management** (Requests & Limits)
- **Health Checks** (Liveness, Readiness & Startup Probes)

---

# Requests vs Limits

Every container can define **resource requests** and **resource limits** for CPU and memory.

Example:

```yaml
resources:
  requests:
    cpu: "250m"
    memory: "256Mi"
  limits:
    cpu: "500m"
    memory: "512Mi"
```

---

## Resource Requests

A **request** is the **minimum amount of CPU or memory** that Kubernetes reserves for a container.

The scheduler uses requests to decide **which node has enough available resources** to run the Pod.

### Purpose

- Used during Pod scheduling.
- Guarantees minimum resources.
- Prevents scheduling on overloaded nodes.

Example:

```yaml
requests:
  cpu: "250m"
  memory: "256Mi"
```

This means the scheduler will only place the Pod on a node with at least:

- 250 millicores of CPU
- 256 MiB of memory available

---

## Resource Limits

A **limit** is the **maximum amount of CPU or memory** a container is allowed to use.

The container runtime enforces these limits while the application is running.

Example:

```yaml
limits:
  cpu: "500m"
  memory: "512Mi"
```

The container cannot use more than:

- 500 millicores of CPU
- 512 MiB of memory

---

# Requests vs Limits Comparison

| Requests | Limits |
|----------|--------|
| Minimum guaranteed resources | Maximum allowed resources |
| Used by the scheduler | Enforced by the container runtime |
| Determines Pod placement | Prevents excessive resource usage |
| Not a usage cap | Acts as a usage cap |

---

# What Happens When CPU Limits Are Exceeded?

CPU is a **compressible resource**.

If a container tries to use more CPU than its limit:

- Kubernetes **does not kill the container**.
- The container is **throttled**.
- The application runs more slowly until CPU usage drops below the limit.

Example:

```
CPU Limit = 500m

Application requests 900m

↓

Container continues running

↓

CPU usage is throttled to 500m
```

---

# What Happens When Memory Limits Are Exceeded?

Memory is **not** a compressible resource.

If a container exceeds its memory limit:

- The Linux kernel terminates the container.
- Kubernetes reports the container as **OOMKilled** (Out Of Memory).
- Depending on the restart policy, Kubernetes starts the container again.

Example:

```
Memory Limit = 512Mi

Application allocates 700Mi

↓

Kernel kills container

↓

Status = OOMKilled
```

---

# Pod Pending Due to Resource Requests

If a Pod requests more CPU or memory than any node can provide, Kubernetes cannot schedule it.

The Pod remains in the **Pending** state.

Example:

```
Node Capacity

CPU: 2 cores
Memory: 4Gi

↓

Pod Requests

CPU: 4 cores
Memory: 8Gi

↓

Pod Status = Pending
```

---

# Kubernetes Probes

Probes allow Kubernetes to monitor the health and availability of containers.

There are three types:

- Liveness Probe
- Readiness Probe
- Startup Probe

---

# Liveness Probe

A **Liveness Probe** checks whether a container is still running correctly.

If the probe fails repeatedly:

- Kubernetes assumes the application is stuck or unhealthy.
- The container is restarted automatically.

Use when:

- Application hangs
- Deadlocks occur
- Recovery requires a restart

Example:

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10
```

---

# Readiness Probe

A **Readiness Probe** checks whether the application is ready to receive traffic.

If it fails:

- The container keeps running.
- Kubernetes removes the Pod from the Service endpoints.
- No new traffic is sent until the probe succeeds again.

Use when:

- Waiting for database connections
- Loading configuration
- Warming caches

Example:

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
```

---

# Startup Probe

A **Startup Probe** checks whether a slow-starting application has finished starting.

Until the startup probe succeeds:

- Liveness and readiness probes are ignored.
- Kubernetes gives the application extra time to initialize.

Useful for:

- Java applications
- Large databases
- Applications with long startup times

Example:

```yaml
startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

---

# Probe Comparison

| Probe | Purpose | Failure Result |
|--------|---------|----------------|
| Liveness | Checks if the container is alive | Container is restarted |
| Readiness | Checks if the application can receive traffic | Pod is removed from Service endpoints |
| Startup | Checks if the application has finished starting | Startup continues until timeout; if it never succeeds, the container is restarted |

---

# Resource Management Flow

```
Pod Created
      │
      ▼
Scheduler checks Requests
      │
      ▼
Pod scheduled to a suitable Node
      │
      ▼
Container starts
      │
      ▼
Runtime enforces Limits
      │
      ▼
CPU → Throttled if limit exceeded
Memory → OOMKilled if limit exceeded
```

---

# Screenshots to Include

## 1. OOMKilled Container

Run:

```bash
kubectl get pods
```

Example:

```
NAME          READY   STATUS      RESTARTS
memory-test   0/1     OOMKilled   3
```

You can also capture:

```bash
kubectl describe pod memory-test
```

to show the **OOMKilled** event.

---

## 2. Pending Pod

Run:

```bash
kubectl get pods
```

Example:

```
NAME          READY   STATUS
large-pod     0/1     Pending
```

Then inspect why:

```bash
kubectl describe pod large-pod
```

Look for events such as:

```
0/2 nodes are available:
Insufficient cpu
Insufficient memory
```

---

## 3. Probe Events

Describe a Pod with probes configured:

```bash
kubectl describe pod <pod-name>
```

Capture the **Events** section.

Example:

```
Warning  Unhealthy
Liveness probe failed

Normal   Killing
Container failed liveness probe

Normal   Started
Started container
```

or

```
Warning  Unhealthy
Readiness probe failed
```

These events demonstrate how Kubernetes reacts when health checks fail.

---

# Summary

- **Requests** reserve the minimum CPU and memory needed for scheduling.
- **Limits** define the maximum CPU and memory a container can use.
- Exceeding a **CPU limit** results in throttling, not termination.
- Exceeding a **memory limit** results in the container being **OOMKilled**.
- Pods remain **Pending** if no node can satisfy their resource requests.
- **Liveness probes** restart unhealthy containers.
- **Readiness probes** control whether a Pod receives traffic.
- **Startup probes** allow slow-starting applications to initialize before liveness and readiness checks begin.
