# Day 59 - Helm Package Manager for Kubernetes

## What is Helm?

Helm is the package manager for Kubernetes. It simplifies deploying, upgrading, configuring, and managing Kubernetes applications using reusable packages called **Helm Charts**.

Instead of writing multiple Kubernetes YAML files manually, Helm allows you to package everything into a chart and deploy it with a single command.

---

## Three Core Concepts of Helm

### 1. Chart
A **Chart** is a collection of Kubernetes resource templates and configuration files.

Example:
```
my-app/
├── Chart.yaml
├── values.yaml
├── templates/
└── charts/
```

---

### 2. Release
A **Release** is an installed instance of a chart.

Example:

```bash
helm install my-release ./my-app
```

- Chart → Blueprint
- Release → Running deployment created from that blueprint

---

### 3. Repository
A **Repository** stores and shares Helm charts.

Example:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

---

# Installing Applications with Helm

Install a chart:

```bash
helm install my-release bitnami/nginx
```

Install using a local chart:

```bash
helm install my-release ./my-app
```

List releases:

```bash
helm list
```

---

# Customizing Helm Charts

You can customize a deployment using:

- values.yaml
- custom-values.yaml
- --set command line option

Example:

```bash
helm install my-release bitnami/nginx \
-f custom-values.yaml
```

or

```bash
helm install my-release bitnami/nginx \
--set replicaCount=3
```

---

# Upgrading a Release

Upgrade using new values:

```bash
helm upgrade my-release bitnami/nginx \
-f custom-values.yaml
```

View release history:

```bash
helm history my-release
```

---

# Rolling Back a Release

Rollback to the previous version:

```bash
helm rollback my-release 1
```

Rollback is useful when an upgrade introduces problems.

---

# Helm Chart Structure

```
my-app/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── serviceaccount.yaml
│   └── NOTES.txt
└── .helmignore
```

### Chart.yaml

Contains metadata about the chart.

Example:

```yaml
apiVersion: v2
name: my-app
description: Sample Helm Chart
type: application
version: 0.1.0
appVersion: "1.0"
```

---

### values.yaml

Stores default configuration values.

---

### templates/

Contains Kubernetes manifest templates.

---

### charts/

Stores chart dependencies.

---

### .helmignore

Specifies files to ignore while packaging the chart.

---

# Go Templating in Helm

Helm uses the Go Template engine to generate Kubernetes manifests dynamically.

Example:

```yaml
replicas: {{ .Values.replicaCount }}
```

This value comes from:

```yaml
replicaCount: 3
```

Another example:

```yaml
image:
  repository: {{ .Values.image.repository }}
  tag: {{ .Values.image.tag }}
```

When Helm renders the chart, placeholders are replaced with values from `values.yaml` or a custom values file.

---

# custom-values.yaml

```yaml
replicaCount: 3

service:
  type: NodePort

resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
  requests:
    cpu: "250m"
    memory: "256Mi"
```

### Explanation

**replicaCount: 3**

Runs three Pod replicas for high availability.

**service.type: NodePort**

Exposes the application on a port accessible from outside the cluster through each worker node.

**resources.requests**

Defines the minimum CPU and memory reserved for the container.

- CPU: 250m
- Memory: 256Mi

**resources.limits**

Defines the maximum CPU and memory the container can consume.

- CPU: 500m
- Memory: 512Mi

---

# Common Helm Commands

```bash
helm version
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo bitnami
helm create my-app
helm lint my-app
helm template my-app
helm install my-release ./my-app
helm list
helm get values my-release
helm upgrade my-release ./my-app -f custom-values.yaml
helm history my-release
helm rollback my-release 1
helm uninstall my-release
```

---

# Key Takeaways

- Helm is the package manager for Kubernetes.
- A Chart is a package containing Kubernetes templates.
- A Release is a deployed instance of a chart.
- Repositories store Helm charts.
- `values.yaml` provides default configuration.
- `custom-values.yaml` overrides default values.
- Go templates make Kubernetes manifests dynamic.
- Helm supports installation, upgrades, rollbacks, and uninstallation with simple commands.
