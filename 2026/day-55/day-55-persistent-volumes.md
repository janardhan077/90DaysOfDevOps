# Day 55 – Kubernetes Persistent Volumes (PV) & Persistent Volume Claims (PVC)

# Persistent Storage in Kubernetes

Containers are **ephemeral**, meaning their data is lost when the container is deleted or recreated. This is acceptable for stateless applications, but stateful applications such as databases require data to persist.

Examples of applications needing persistent storage:
- MySQL
- PostgreSQL
- MongoDB
- WordPress uploads
- Jenkins home directory

Without persistent storage:
- Database data is lost after Pod deletion.
- User-uploaded files disappear.
- Application state cannot be recovered.

Persistent storage ensures data survives Pod restarts, rescheduling, and recreation.

---

# Why Containers Need Persistent Storage

By default, container filesystems are temporary.

For example:

1. A MySQL container stores database files.
2. The Pod is deleted.
3. Kubernetes creates a new Pod.
4. The new container starts with an empty filesystem.

Result:
- All database data is lost.

Persistent storage stores data outside the container so it remains available even when Pods change.

---

# What is a Persistent Volume (PV)?

A **Persistent Volume (PV)** is a storage resource in a Kubernetes cluster.

It is created by:
- A cluster administrator
- Or automatically through a StorageClass

A PV represents actual storage such as:
- Local disk
- NFS
- AWS EBS
- Azure Disk
- Google Persistent Disk
- Ceph
- Other storage systems

Think of a PV as a **physical storage device made available to the cluster**.

---

# What is a Persistent Volume Claim (PVC)?

A **Persistent Volume Claim (PVC)** is a request for storage made by an application.

Instead of asking for a specific disk, the application requests:
- Storage size
- Access mode
- Storage class (optional)

Kubernetes finds a suitable PV and binds it to the PVC.

Example request:

- 10 GiB storage
- ReadWriteOnce access

The application only uses the PVC and does not need to know where the storage actually resides.

---

# Relationship Between PV and PVC

```
Application
      │
      ▼
Persistent Volume Claim (PVC)
      │
      ▼
Persistent Volume (PV)
      │
      ▼
Actual Storage
(Local Disk / NFS / Cloud Disk)
```

- **PV** provides the storage.
- **PVC** requests the storage.
- Pods mount the **PVC**, not the PV directly.

---

# Static Provisioning

In **static provisioning**, the administrator creates Persistent Volumes manually before applications request them.

Process:

1. Administrator creates one or more PVs.
2. Application creates a PVC.
3. Kubernetes matches the PVC with an available PV.

### Advantages

- Full control over storage.
- Simple for small environments.

### Disadvantages

- Manual management.
- Does not scale well for many applications.

---

# Dynamic Provisioning

In **dynamic provisioning**, Kubernetes automatically creates a Persistent Volume when a PVC is created.

This requires a **StorageClass**.

Process:

1. Application creates a PVC.
2. Kubernetes checks the StorageClass.
3. Storage is automatically created.
4. The new PV is bound to the PVC.

### Advantages

- Automatic storage creation.
- Better scalability.
- Common in cloud environments.

### Disadvantages

- Requires a configured StorageClass.
- Less manual control over individual volumes.

---

# Static vs Dynamic Provisioning

| Static Provisioning | Dynamic Provisioning |
|---------------------|----------------------|
| PV created manually | PV created automatically |
| Admin manages storage | Kubernetes manages storage |
| Suitable for small clusters | Suitable for large and cloud environments |
| No StorageClass required | Requires a StorageClass |

---

# Access Modes

Access modes define **how a volume can be mounted by Pods**.

## ReadWriteOnce (RWO)

- Mounted as read-write by **one node** at a time.
- Most common access mode.
- Used by many cloud block storage systems.

Example:
- MySQL
- PostgreSQL

---

## ReadOnlyMany (ROX)

- Multiple nodes can mount the volume.
- Data can only be read.

Useful for:
- Shared configuration
- Static content

---

## ReadWriteMany (RWX)

- Multiple nodes can read and write simultaneously.

Useful for:
- Shared file storage
- Content management systems
- Shared application data

Typically supported by:
- NFS
- CephFS
- Azure Files

---

## ReadWriteOncePod (RWOP)

- The volume can be mounted as read-write by **only one Pod** in the cluster.

Useful for applications that require exclusive access.

---

# Access Mode Summary

| Access Mode | Description |
|--------------|-------------|
| ReadWriteOnce (RWO) | One node can read and write |
| ReadOnlyMany (ROX) | Multiple nodes can read only |
| ReadWriteMany (RWX) | Multiple nodes can read and write |
| ReadWriteOncePod (RWOP) | Only one Pod can read and write |

---

# Reclaim Policies

A **reclaim policy** determines what happens to a Persistent Volume after its PVC is deleted.

## Retain

The PV and its data are preserved.

- Data remains intact.
- Administrator must manually clean up or reuse the volume.

Best for:
- Important databases
- Critical production data

---

## Delete

The PV and underlying storage are automatically deleted when the PVC is removed.

Best for:
- Temporary workloads
- Development and testing environments

---

## Recycle (Deprecated)

Previously:
- Data was erased.
- PV was made available for reuse.

This policy is **deprecated** and should not be used.

---

# Reclaim Policy Summary

| Policy | Behavior |
|----------|----------|
| Retain | Keeps the storage and data after PVC deletion |
| Delete | Removes the storage and data automatically |
| Recycle | Cleans and reuses the volume (deprecated) |

---

# Summary

- Containers are ephemeral, so they need persistent storage for important data.
- A **Persistent Volume (PV)** provides storage to the cluster.
- A **Persistent Volume Claim (PVC)** requests storage for an application.
- Pods use **PVCs**, which are bound to matching **PVs**.
- **Static provisioning** requires manually creating PVs.
- **Dynamic provisioning** automatically creates PVs using a StorageClass.
- Access modes define how volumes can be mounted by Pods.
- Reclaim policies determine what happens to storage after a PVC is deleted.
