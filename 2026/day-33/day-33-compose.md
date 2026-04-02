# Day 33 – Docker Compose Basics

Today I learned how to use Docker Compose to run multi-container applications.

## Docker Compose

- Tool to manage multiple containers
- Uses docker-compose.yml file
- Start all services with one command

---

## First Compose File

- Created compose file for Nginx
- Used docker compose up
- Accessed container in browser
- Stopped using docker compose down

---

## Multi-Container Setup

- Ran WordPress + MySQL
- Used service names for communication
- Added volume for MySQL data persistence

---

## Compose Commands

- docker compose up -d
- docker compose down
- docker compose ps
- docker compose logs
- docker compose logs -f

---

## Environment Variables

- Added variables in compose file
- Used .env file
- Improved configuration management

---

## Summary

Today I learned:

- Running multiple containers using Compose
- Container communication using service names
- Managing services with simple commands
- Importance of volumes in multi-container apps
