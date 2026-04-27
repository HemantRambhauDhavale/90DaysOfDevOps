# Day 45 – Docker Build & Push in GitHub Actions

Today I built an end-to-end CI/CD workflow for Docker images.

## Preparation

- Used app with Dockerfile
- Added DOCKER_USERNAME secret
- Added DOCKER_TOKEN secret

---

## Workflow Steps

- Trigger on push to main
- Checkout code
- Build Docker image
- Login to Docker Hub
- Push image automatically

---

## Tags Used

- latest
- short commit SHA

---

## Conditions

- Push image only on main branch
- Feature branches only test build

---

## README Badge

- Added workflow status badge
- Shows pipeline pass/fail state

---

## Final Flow

git push → workflow starts → image builds → image pushed → pull & run

---

## Summary

Today I learned:

- Docker automation with GitHub Actions
- Secure credential usage
- Image version tagging
- Real CI/CD deployment flow
