# Day 52 - Kubernetes Namespaces and Deployments

## What are Namespaces and Why Use Them?

Namespaces are a way to divide a Kubernetes cluster into multiple virtual environments. They help organize and isolate resources within the same cluster.

### Why use Namespaces?

* Separate development, testing, and production environments.
* Avoid naming conflicts between resources.
* Apply resource quotas and access controls.
* Improve cluster organization and management.

### Example

```bash
kubectl create namespace dev
kubectl create namespace test
kubectl create namespace prod
```

A Pod named `nginx` can exist in both the `dev` and `prod` namespaces without conflict.

---

# Deployment Manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

## Explanation of Each Section

### apiVersion

```yaml
apiVersion: apps/v1
```

Specifies the Kubernetes API version used by the Deployment.

### kind

```yaml
kind: Deployment
```

Defines the resource type as a Deployment.

### metadata

```yaml
metadata:
  name: nginx-deployment
  namespace: dev
```

Contains identifying information such as the Deployment name and namespace.

### spec

```yaml
spec:
```

Defines the desired state of the Deployment.

### replicas

```yaml
replicas: 3
```

Specifies that three Pod replicas should be running.

### selector

```yaml
selector:
  matchLabels:
    app: nginx
```

Tells the Deployment which Pods it manages.

### template

```yaml
template:
```

Defines the Pod template used to create new Pods.

### containers

```yaml
containers:
- name: nginx
  image: nginx:latest
```

Specifies the container image and configuration.

### ports

```yaml
ports:
- containerPort: 80
```

Exposes port 80 inside the container.

---

# Deployment Pod vs Standalone Pod

## Standalone Pod

```bash
kubectl delete pod nginx
```

Result:

* The Pod is permanently removed.
* Kubernetes does not recreate it automatically.

## Deployment Managed Pod

```bash
kubectl delete pod nginx-deployment-xxxx
```

Result:

* Deployment notices the missing Pod.
* A new replacement Pod is automatically created.
* Desired replica count is maintained.

### Key Difference

| Standalone Pod                 | Deployment Pod             |
| ------------------------------ | -------------------------- |
| Deleted permanently            | Recreated automatically    |
| No self-healing                | Self-healing enabled       |
| Not recommended for production | Recommended for production |

---

# Scaling Deployments

Scaling changes the number of Pod replicas running.

## Imperative Scaling

Increase replicas using a command:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Verify:

```bash
kubectl get pods
```

Kubernetes creates additional Pods until five replicas are running.

---

## Declarative Scaling

Modify the Deployment manifest:

```yaml
spec:
  replicas: 5
```

Apply the changes:

```bash
kubectl apply -f deployment.yaml
```

Kubernetes updates the Deployment to match the desired state.

---

# Rolling Updates

A rolling update gradually replaces old Pods with new Pods without downtime.

Example:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.27
```

What happens:

1. New Pods are created.
2. Traffic is shifted to new Pods.
3. Old Pods are terminated.
4. Application remains available during the update.

Check rollout status:

```bash
kubectl rollout status deployment/nginx-deployment
```

---

# Rollbacks

If the new version causes problems, Kubernetes can revert to the previous version.

View rollout history:

```bash
kubectl rollout history deployment/nginx-deployment
```

Rollback:

```bash
kubectl rollout undo deployment/nginx-deployment
```

What happens:

* Kubernetes restores the previous ReplicaSet.
* Old working Pods are recreated.
* Faulty version is removed.

---

# Screenshots

## Deployment Running

Capture the output of:

```bash
kubectl get deployments -n dev
```

Screenshot: *Insert screenshot here*

---

## Pods Running

Capture the output of:

```bash
kubectl get pods -n dev
```

Screenshot: *Insert screenshot here*

---

# Summary

Today I learned how Kubernetes Namespaces help organize resources within a cluster and how Deployments provide self-healing, scaling, rolling updates, and rollback capabilities. Deployments ensure application availability by automatically maintaining the desired number of Pod replicas and replacing failed Pods when necessary.
