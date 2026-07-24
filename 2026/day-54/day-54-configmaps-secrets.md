# Day 54 – Kubernetes ConfigMaps & Secrets

# ConfigMaps and Secrets in Kubernetes

## What is a ConfigMap?

A **ConfigMap** is a Kubernetes object used to store **non-sensitive configuration data** as key-value pairs.

Examples:
- Application configuration
- Feature flags
- URLs
- Log levels
- Configuration files

### Example

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_NAME: myapp
  LOG_LEVEL: debug
```

---

## What is a Secret?

A **Secret** is a Kubernetes object used to store **sensitive information**.

Examples:
- Database passwords
- API keys
- OAuth tokens
- SSH keys
- TLS certificates

Secrets are Base64 encoded before being stored.

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: dXNlcg==
  password: cGFzc3dvcmQ=
```

---

# When to Use ConfigMaps vs Secrets

| ConfigMap | Secret |
|------------|---------|
| Non-sensitive data | Sensitive data |
| Plain text | Base64 encoded |
| Configuration | Passwords, tokens, keys |
| Safe to expose | Should be protected |

### Use ConfigMaps for

- Application settings
- URLs
- Port numbers
- Feature flags
- Config files

### Use Secrets for

- Database credentials
- API tokens
- Certificates
- SSH keys
- Private keys

---

# Environment Variables vs Volume Mounts

ConfigMaps and Secrets can be injected into Pods in two ways.

## 1. Environment Variables

Example:

```yaml
env:
- name: DB_HOST
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: DB_HOST
```

### Advantages

- Easy to use
- Good for small values
- Accessed directly by applications

### Limitation

Environment variables are loaded **only when the container starts**.

If the ConfigMap or Secret changes:

- Running Pods **do not** receive the update.
- Pod restart is required.

---

## 2. Volume Mounts

Example

```yaml
volumes:
- name: config-volume
  configMap:
    name: app-config

volumeMounts:
- name: config-volume
  mountPath: /etc/config
```

Each key becomes a file inside the mounted directory.

Example:

```
/etc/config/
    APP_NAME
    LOG_LEVEL
```

### Advantages

- Supports configuration files
- Changes can automatically appear without restarting the Pod
- Better for large configuration files

---

# Environment Variables vs Volume Mounts

| Environment Variables | Volume Mounts |
|-----------------------|---------------|
| Loaded once at container startup | Mounted as files |
| Requires Pod restart after updates | Updates automatically |
| Best for small values | Best for config files |
| Accessed through process environment | Accessed as files |

---

# Why Base64 is Encoding, Not Encryption

Many beginners think Kubernetes Secrets are encrypted because they contain Base64 text.

Example:

```
password = cGFzc3dvcmQ=
```

This is **NOT encryption**.

It is simply **Base64 encoding**.

Anyone can decode it using:

```bash
echo "cGFzc3dvcmQ=" | base64 --decode
```

Output:

```
password
```

---

## Encoding

Encoding converts data into another format so computers can safely transmit or store it.

Properties:

- Easily reversible
- No secret key required
- Not intended for security

Examples:

- Base64
- URL Encoding

---

## Encryption

Encryption protects data using an encryption key.

Properties:

- Requires a secret key
- Cannot be read without the key
- Designed for security

Examples:

- AES
- RSA

---

## Comparison

| Base64 Encoding | Encryption |
|-----------------|------------|
| Reversible by anyone | Requires key |
| Not secure | Secure |
| Used for data formatting | Used for protection |
| No confidentiality | Provides confidentiality |

---

# How ConfigMap Updates Propagate

Suppose a ConfigMap contains:

```yaml
LOG_LEVEL=INFO
```

Application mounts it as a volume.

Later it is updated to:

```yaml
LOG_LEVEL=DEBUG
```

Kubernetes automatically refreshes the mounted files after a short delay (typically within a minute or two).

The application can read the updated file without restarting the Pod (if it watches or reloads the file).

---

## Using Environment Variables

If the same ConfigMap is used as an environment variable:

```yaml
env:
- name: LOG_LEVEL
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: LOG_LEVEL
```

The application receives:

```
LOG_LEVEL=INFO
```

Even if the ConfigMap changes to:

```
LOG_LEVEL=DEBUG
```

the running container **still sees `INFO`** until the Pod is restarted.

---

# Summary

- **ConfigMaps** store non-sensitive configuration data.
- **Secrets** store sensitive data such as passwords and API keys.
- ConfigMaps and Secrets can be injected as **environment variables** or **mounted volumes**.
- Environment variables **do not update automatically** after Pod startup.
- Mounted ConfigMap volumes **can reflect updates automatically**.
- Base64 is **encoding**, not **encryption**, and does not provide security.
- Use ConfigMaps for application settings and Secrets for confidential information.
