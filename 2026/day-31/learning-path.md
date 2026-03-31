# Day 31 – Dockerfile & Custom Images

Today I learned how to create custom Docker images using Dockerfile.

## First Dockerfile

- Used ubuntu as base image
- Installed curl
- Added default command
- Built image and ran container

---

## Dockerfile Instructions

- FROM → base image
- RUN → execute commands
- COPY → copy files
- WORKDIR → set working directory
- EXPOSE → define port
- CMD → default command

---

## CMD vs ENTRYPOINT

- CMD → can be overridden
- ENTRYPOINT → fixed command

---

## Web Application

- Created index.html
- Used nginx:alpine
- Copied file to container
- Ran container and accessed in browser

---

## .dockerignore

- Ignored unnecessary files
- Reduced image size

---

## Build Optimization

- Docker uses caching
- Layer order matters
- Frequently changing steps should be last

---

## Summary

Today I learned:

- Writing Dockerfile
- Building custom images
- Difference between CMD and ENTRYPOINT
- Docker build optimization
