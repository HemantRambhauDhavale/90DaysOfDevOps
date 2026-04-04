# Day 35 – Multi-Stage Builds & Docker Hub

Today I learned how to optimize Docker images and share them using Docker Hub.

## Single Stage Build

- Built image using single Dockerfile
- Image size was large

---

## Multi-Stage Build

- Used multiple stages in Dockerfile
- Built app in first stage
- Copied final output to minimal image
- Reduced image size significantly

---

## Docker Hub

- Created account
- Logged in using docker login
- Tagged image
- Pushed image using docker push
- Pulled image to verify

---

## Docker Hub Repository

- Checked repository on Docker Hub
- Explored tags
- Understood versioning

---

## Best Practices

- Used minimal base image (alpine)
- Avoided root user
- Combined RUN commands
- Used specific tags instead of latest

---

## Summary

Today I learned:

- Multi-stage Docker builds
- Image optimization
- Docker Hub usage
- Best practices for Docker images
