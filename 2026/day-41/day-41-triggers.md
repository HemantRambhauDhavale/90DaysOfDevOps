# Day 41 – Triggers & Matrix Builds

Today I learned advanced GitHub Actions workflow execution methods.

## Workflow Triggers

- push trigger
- pull_request trigger
- schedule trigger using cron
- workflow_dispatch manual trigger

---

## Pull Request Workflow

- Runs when PR is opened or updated
- Useful for validation before merge

---

## Scheduled Workflow

- Runs automatically at fixed times
- Useful for maintenance jobs

---

## Manual Workflow

- Triggered from Actions tab
- Accepts input values

---

## Matrix Builds

- Ran same job across Python versions:
  - 3.10
  - 3.11
  - 3.12

- Extended to multiple OS

---

## Advanced Matrix Options

- exclude specific combinations
- fail-fast true vs false

---

## Summary

Today I learned:

- Multiple workflow triggers
- Parallel jobs using matrix builds
- Better automation using GitHub Actions
