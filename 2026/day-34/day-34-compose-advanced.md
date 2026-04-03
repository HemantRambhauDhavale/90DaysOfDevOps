# Day 34 – Advanced Docker Compose

Today I worked on building real-world multi-container applications using Docker Compose.

## Multi-Service Setup

- Web app (custom Dockerfile)
- Database (MySQL/Postgres)
- Cache (Redis)

---

## depends_on & Healthchecks

- Used depends_on for startup order
- Added healthcheck for database
- Used condition: service_healthy
- Ensured app waits for DB readiness

---

## Restart Policies

- restart: always → always restarts container
- restart: on-failure → restarts only on crash

---

## Build from Dockerfile

- Used build: in compose file
- Built custom app image
- Rebuilt using docker compose up --build

---

## Networks & Volumes

- Defined custom networks
- Defined named volumes
- Improved organization and persistence

---

## Scaling

- Used docker compose up --scale
- Observed port conflicts
- Learned need for load balancing

---

## Summary

Today I learned:

- Real-world Docker Compose usage
- Service dependencies and readiness
- Restart behavior
- Scaling limitations and challenges
