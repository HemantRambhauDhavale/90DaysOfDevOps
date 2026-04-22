# Day 43 – Jobs, Steps, Env Vars & Conditionals

Today I learned advanced workflow control in GitHub Actions.

## Multi-Job Workflows

- build job
- test job depends on build
- deploy job depends on test

Used:
needs:

---

## Environment Variables

- Workflow level variables
- Job level variables
- Step level variables

Used GitHub context:
- actor
- commit SHA

---

## Job Outputs

- Created output in first job
- Used output in second job

Useful for passing data between jobs.

---

## Conditionals

- Run step only on main branch
- Run step after failure
- Run job only on push event
- Used continue-on-error

---

## Smart Pipeline

- lint and test in parallel
- summary after both complete

---

## Summary

Today I learned:

- Workflow dependencies
- Sharing data between jobs
- Conditional execution
- Smarter CI/CD design
