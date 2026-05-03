# Day 48 – GitHub Actions Capstone Project

Today I built a complete CI/CD pipeline.

## Project Setup

- Created app with Dockerfile
- Added basic test

---

## Reusable Workflows

### Build & Test
- Installs dependencies
- Runs tests
- Outputs test result

### Docker Build
- Builds image
- Pushes to Docker Hub
- Outputs image URL

---

## PR Pipeline

- Runs on pull_request
- Calls build-test workflow
- No Docker push

---

## Main Pipeline

- Runs on push to main
- Step 1: Test
- Step 2: Build & Push Docker image
- Step 3: Deploy

---

## Scheduled Workflow

- Runs every 12 hours
- Pulls image
- Runs container
- Performs health check

---

## Pipeline Flow

PR → test  
Main → test → build → push → deploy  
Schedule → health check  

---

## Summary

Today I learned:

- Complete CI/CD pipeline design
- Workflow orchestration
- Combining multiple workflows
- Real-world DevOps implementation
