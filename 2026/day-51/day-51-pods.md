# Day 51 – Kubernetes Pods

## 1. Four Required Fields of a Kubernetes Manifest

### apiVersion

Specifies which Kubernetes API version should be used to create the resource.

Example:

```yaml
apiVersion: v1
```

### kind

Defines the type of Kubernetes resource being created.

Example:

```yaml
kind: Pod
```

### metadata

Contains information that identifies the resource such as name and labels.

Example:

```yaml
metadata:
  name: nginx-pod
  labels:
    app: nginx
```

### spec

Describes the desired state of the resource, including containers, images, ports, volumes, and other configurations.

Example:

```yaml
spec:
  containers:
  - name: nginx
    image: nginx:latest
```

---

## 2. Nginx Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

---

## 3. BusyBox Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-pod
  labels:
    app: busybox
    environment: dev
spec:
  containers:
  - name: busybox
    image: busybox:latest
    command: ["sh", "-c", "echo Hello from BusyBox && sleep 3600"]
```

---

## 4. Third Pod Manifest (Labels Practice)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: apache-pod
  labels:
    app: apache
    environment: test
    team: devops
spec:
  containers:
  - name: apache
    image: httpd:latest
    ports:
    - containerPort: 80
```

---

## 5. Imperative vs Declarative

### Imperative Approach

Commands are executed directly from the terminal.

Example:

```bash
kubectl run redis-pod --image=redis:latest
```

Characteristics:

* Quick for testing
* No YAML file required
* Harder to track changes
* Not ideal for production

### Declarative Approach

Resources are defined in YAML files and applied using kubectl.

Example:

```bash
kubectl apply -f nginx-pod.yaml
```

Characteristics:

* Infrastructure as Code
* Easy to version control with Git
* Reproducible
* Preferred for production environments

| Imperative            | Declarative         |
| --------------------- | ------------------- |
| Command-based         | YAML-based          |
| Fast for testing      | Best for production |
| Hard to maintain      | Easy to maintain    |
| Limited repeatability | Highly repeatable   |

---

## 6. Screenshot of Running Pods

Insert screenshot showing:

```bash
kubectl get pods
```

Output should show:

```text
NAME          READY   STATUS    RESTARTS   AGE
nginx-pod     1/1     Running   0          xx
busybox-pod   1/1     Running   0          xx
apache-pod    1/1     Running   0          xx
```

---

## 7. What Happens When You Delete a Standalone Pod?

A standalone Pod is not managed by any controller.

When a Pod is deleted:

```bash
kubectl delete pod nginx-pod
```

Kubernetes permanently removes the Pod.

No new Pod is automatically created because there is no Deployment or ReplicaSet managing it.

This demonstrates why standalone Pods are mainly used for learning and debugging purposes. In production environments, Deployments are preferred because they provide self-healing, scaling, rolling updates, and automatic Pod recreation.

---

## Key Learnings

* Pods are the smallest deployable unit in Kubernetes.
* Every manifest requires apiVersion, kind, metadata, and spec.
* Logs capture stdout and stderr from containers.
* Labels help organize and filter resources.
* Declarative resource management is preferred over imperative commands.
* Standalone Pods do not self-heal after deletion.
