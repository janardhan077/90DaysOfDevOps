# Day 37 – Docker Revision & Self-Check

## Self-Assessment Checklist
Mark as: ✅ Can do | ⚠️ Shaky | ❌ Haven't done

- [ ] Run a container from Docker Hub
- [ ] List, stop, remove containers and images
- [ ] Explain image layers and caching
- [ ] Write a Dockerfile from scratch
- [ ] Explain CMD vs ENTRYPOINT
- [ ] Build and tag a custom image
- [ ] Create and use named volumes
- [ ] Use bind mounts
- [ ] Create custom networks
- [ ] Write docker-compose.yml
- [ ] Use environment variables and `.env`
- [ ] Write multi-stage Dockerfile
- [ ] Push image to Docker Hub
- [ ] Use healthchecks and depends_on

## Quick-Fire Answers

### 1) Difference between image and container?
- **Image** = blueprint/template
- **Container** = running instance of image

### 2) What happens to data when container is removed?
- Container data is lost unless stored in **volume** or **bind mount**.

### 3) How do two containers on same custom network communicate?
- They communicate using **container names as hostnames**.

### 4) `docker compose down -v` vs `docker compose down`
- `down` → removes containers + networks
- `down -v` → also removes named volumes

### 5) Why multi-stage builds?
- Smaller images, better security, only final artifacts included.

### 6) COPY vs ADD
- `COPY` → simple file copy
- `ADD` → copy + extract tar + supports URL

### 7) What does `-p 8080:80` mean?
- Host port **8080** maps to container port **80**.

### 8) Check Docker disk usage
- `docker system df`

## Weak Spots to Revisit
- Topic 1: __________________
- Topic 2: __________________

## Hands-On Redo Notes
Write what you practiced again today:
- __________________________________
- __________________________________
- __________________________________
