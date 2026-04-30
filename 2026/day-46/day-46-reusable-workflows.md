# Day 46 – Reusable Workflows & Composite Actions

Today I learned how to reuse GitHub Actions workflows.

## Reusable Workflow

- Created workflow with workflow_call
- Accepted inputs:
  - app_name
  - environment
- Accepted secrets:
  - docker_token

---

## Caller Workflow

- Triggered on push
- Called reusable workflow using uses:
- Passed inputs and secrets

---

## Outputs

- Generated build version
- Passed output to next job
- Used needs to access output

---

## Composite Action

- Created custom action using action.yml
- Defined inputs and outputs
- Printed greeting and system info
- Used with uses: in workflow

---

## Comparison

Reusable Workflow:
- Job-level reuse
- Can have multiple jobs

Composite Action:
- Step-level reuse
- Used inside a job

---

## Summary

Today I learned:

- Workflow reuse using workflow_call
- Passing inputs and outputs
- Creating custom GitHub actions
- Writing modular CI/CD pipelines
