# Day 29 – Introduction to Docker

Today I learned the basics of Docker and containerization.

## What is Docker

- Docker is a tool to run applications in containers
- Containers include application + dependencies
- Solves "it works on my machine" problem

---

## Containers vs Virtual Machines

Containers:
- Lightweight
- Fast
- Share host OS kernel

Virtual Machines:
- Heavy
- Run full OS
- Slower compared to containers

---

## Docker Architecture

- Docker Client → sends commands
- Docker Daemon → manages containers
- Docker Images → templates
- Docker Containers → running instances
- Docker Registry (Docker Hub) → stores images

---

## Practice Done

- Installed Docker
- Ran hello-world container
- Ran Nginx container
- Ran Ubuntu container in interactive mode

---

## Docker Commands Practiced

- docker run
- docker ps
- docker ps -a
- docker stop
- docker rm

---

## Advanced Usage

- Detached mode (-d)
- Interactive mode (-it)
- Port mapping (-p)
- Naming containers (--name)
- docker logs
- docker exec

---

## Summary

Today I learned:

- Basics of Docker
- How containers work
- Running and managing containers
- Importance of Docker in DevOps
