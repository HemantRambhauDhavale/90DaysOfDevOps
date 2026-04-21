# Day 42 – GitHub Actions Runners

Today I learned how workflow jobs are executed using runners.

## What is a Runner?

- Machine that executes workflow jobs
- Needed for build, test, deploy tasks

---

## GitHub-Hosted Runners

- Managed by GitHub
- Used:
  - ubuntu-latest
  - windows-latest
  - macos-latest

- Pre-installed tools:
  - Docker
  - Python
  - Node.js
  - Git

---

## Self-Hosted Runner

- Installed on own machine or server
- Connected to GitHub repository
- Showed as Idle when online

---

## Workflow on Self-Hosted

- Used runs-on: self-hosted
- Printed hostname
- Created local file

---

## Labels

- Used labels to target specific runner
- Helpful for multiple machines

---

## Summary

Today I learned:

- Difference between hosted and self-hosted runners
- Workflow execution infrastructure
- More control using self-hosted runners
