# 🚀 Day 34 – Docker Compose Advanced (Production-Style Backend Focus)

## 🎯 Goal

Build a **production-like backend system** using Docker Compose with:

* Flask Backend API
* PostgreSQL Database
* Redis Cache
* Healthchecks & Restart Policies
* Custom Dockerfile
* Networks & Volumes

---

## ```

---

## 🧩 Backend (Flask API)

### Features:

* Connects to PostgreSQL
* Uses Redis for caching (visit counter)
* Retry logic for DB connection (important in containers)

### Key Logic:

* On each request → increments Redis counter
* Returns response with visit count

---

## 🧩 Database (PostgreSQL)

### Why used:

* Persistent storage
* Production-grade relational database

### Important Config:

* Environment variables for DB setup
* Volume used for data persistence
* Healthcheck ensures DB is ready before backend starts

---

## 🧩 Cache (Redis)

### Why used:

* Fast in-memory storage
* Reduces load on database
* Used here for counting visits

---

## 🧩 Docker Compose (Core Concepts Used)

### ✅ Service Communication

* Services communicate using **service names**

  * `db` → PostgreSQL
  * `cache` → Redis

### ✅ Healthcheck

* Ensures DB is ready before backend starts
* Prevents connection failures at startup

### ✅ Restart Policy

* `restart: always` ensures high availability

### ✅ Volumes

* `db-data` → persists database even after container removal

### ✅ Networks

* Custom network (`app-network`) for secure communication

---

## 🧩 Run the Project

```bash
docker compose up --build
```

---

## 🧩 Scaling Backend

```bash
docker compose up --scale backend=3
```

### ⚠️ Limitation:

* No load balancing by default

### 💡 Improvement:

* Add Nginx load balancer
* Or move to Kubernetes / Docker Swarm

---

## 🧠 Key Learnings

* Built a **real backend system using containers**
* Understood **service dependency & healthchecks**
* Learned **Redis as caching layer**
* Learned **PostgreSQL with persistent storage**
* Implemented **production-style Docker Compose setup**

---

## 🚀 Final Thoughts

This project simulates a **real-world backend architecture**:

* API Layer (Flask)
* Cache Layer (Redis)
* Database Layer (PostgreSQL)

A strong step towards **production DevOps systems 🔥**

---

#90DaysOfDevOps #Day34 #Backend #Docker #DevOpsJourney

