# Day 30 – Docker Images & Container Lifecycle

Today I learned about Docker images, layers, and container lifecycle.

## Docker Images

- Pulled nginx, ubuntu, alpine images
- Compared image sizes
- Alpine is smaller because it is lightweight
- Inspected images for details

---

## Image Layers

- Used docker image history
- Each image has multiple layers
- Some layers have size, some 0B (metadata)
- Layers help in caching and efficiency

---

## Container Lifecycle

Steps practiced:

- Create container (docker create)
- Start container (docker start)
- Pause container
- Unpause container
- Stop container
- Restart container
- Kill container
- Remove container

Observed container states using docker ps -a

---

## Working with Containers

- Ran container in detached mode (-d)
- Checked logs (docker logs)
- Follow logs (docker logs -f)
- Used docker exec to enter container
- Ran commands inside container
- Inspected container (docker inspect)

---

## Cleanup

- Stopped all containers
- Removed stopped containers
- Removed unused images
- Checked disk usage (docker system df)

---

## Summary

Today I learned:

- How Docker images work
- Importance of layers
- Full lifecycle of containers
- Managing Docker resources
