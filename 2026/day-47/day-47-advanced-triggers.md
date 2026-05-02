# Day 47 – Advanced Triggers

Today I learned advanced event-based triggers in GitHub Actions.

## Pull Request Events

- Triggered on:
  - opened
  - synchronize
  - reopened
  - closed

- Printed PR details:
  - title
  - author
  - branches

---

## PR Validation Workflow

- File size check
- Branch name check
- PR description check

---

## Scheduled Workflows

- Cron-based triggers
- Example:
  - weekly
  - every 6 hours

- Added manual trigger for testing

---

## Path Filters

- Run only when src/ changes
- Ignore docs changes

---

## Workflow Chaining

- Used workflow_run
- Trigger second workflow after first completes

---

## External Trigger

- Used repository_dispatch
- Trigger workflow using API call

---

## Summary

Today I learned:

- Event-driven pipelines
- Advanced trigger conditions
- Workflow chaining
- External pipeline triggers
