# Day 32 – Docker Volumes & Networking

Today I learned about data persistence and container communication in Docker.

## Problem

- Containers are temporary
- Data is lost when container is removed

---

## Named Volumes

- Created volume using docker volume create
- Attached volume to container
- Data persisted even after container removal
- Used docker volume ls and docker volume inspect

---

## Bind Mounts

- Linked local folder with container
- Changes in local file reflected inside container
- Useful for development

---

## Networking Basics

- Listed networks using docker network ls
- Inspected default bridge network
- Containers could not communicate easily by name

---

## Custom Network

- Created network using docker network create
- Ran containers on same network
- Containers communicated using names

---

## Combined Setup

- Used volume + custom network
- Connected app container with database container

---

## Summary

Today I learned:

- Data persistence using volumes
- Difference between volume and bind mount
- Container communication using networks
- Importance of Docker networking in real applications
