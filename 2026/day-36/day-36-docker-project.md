# 🚀 Day 36 – Docker Project: Dockerize a Full Application

## 📌 Project Chosen

**BookMyShow Clone – Full Stack Movie Ticket Booking App**

I chose this project because it is a **real-world multi-container full-stack application** with:

* React frontend
* Node.js backend
* PostgreSQL database
* Redis caching
* Nginx reverse proxy

This project helped me understand how real production applications are Dockerized end-to-end.

---

# 🐳 Dockerfile

## Backend Dockerfile

```dockerfile
# Use lightweight Node image
FROM node:20-alpine

# Create app directory
WORKDIR /app

# Copy dependency files
COPY package*.json ./

# Install production dependencies
RUN npm ci --omit=dev

# Copy source code
COPY . .

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Expose backend port
EXPOSE 5000

# Start application
CMD ["npm", "start"]
```

## Frontend Dockerfile

```dockerfile
# Build stage
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

# 🧩 Docker Compose

```yaml
version: "3.9"

services:
  frontend:
    build: ./frontend
    container_name: bms_frontend
    depends_on:
      - backend
    networks:
      - bms_network

  backend:
    build: ./backend
    container_name: bms_backend
    environment:
      DB_HOST: postgres
      REDIS_HOST: redis
    depends_on:
      postgres:
        condition: service_healthy
    networks:
      - bms_network

  postgres:
    image: postgres:15-alpine
    container_name: bms_postgres
    environment:
      POSTGRES_DB: bookmyshow
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - bms_network

  redis:
    image: redis:alpine
    container_name: bms_redis
    networks:
      - bms_network

  nginx:
    build: ./nginx
    container_name: bms_nginx
    ports:
      - "${APP_PORT:-8080}:80"
    depends_on:
      - frontend
      - backend
    networks:
      - bms_network

volumes:
  postgres_data:

networks:
  bms_network:
```

---

# 🌱 .env

```env
APP_PORT=8080
POSTGRES_DB=bookmyshow
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
```

---

# 🧱 .dockerignore

```dockerignore
node_modules
npm-debug.log
.git
.env
Dockerfile
README.md
```

---

# ⚔️ Challenges Faced

## 1) `npm ci` failed

**Issue:** `package-lock.json` missing.

**Fix:**

```bash
npm install
```

Generated lock file and rebuilt image.

## 2) Port 80 already in use

**Issue:** Nginx container failed.

**Fix:** Used environment-based port:

```env
APP_PORT=8080
```

## 3) Movie posters not loading

**Issue:** External placeholder URLs were failing.

**Fix:** Moved posters into:

```text
frontend/public/posters/
```

And used local static paths.

---

# 📦 Final Image Size

* Backend: ~180MB
* Frontend: ~30MB
* Nginx: ~25MB

Using Alpine + multi-stage significantly reduced image size.

---

# 🐙 Docker Hub

```bash
# Tag
Docker tag bookmyshow-app yourdockerhub/bookmyshow:latest

# Push
Docker push yourdockerhub/bookmyshow:latest
```

**Docker Hub Link:**
`https://hub.docker.com/r/yourdockerhub/bookmyshow`

---

# ▶️ How to Run

```bash
docker compose up -d --build
```

Open:

```text
http://localhost:8080
```

---

# 🧪 Fresh Pull Test

```bash
docker compose down -v

docker system prune -a

docker compose pull

docker compose up -d
```

Verified app works from fresh images.

---

# 🎯 What I Learned

* Multi-stage Docker builds
* Reverse proxy with Nginx
* Compose networking
* Postgres healthchecks
* Persistent volumes
* Environment variable based configuration
* Real-world deployment workflow

---


