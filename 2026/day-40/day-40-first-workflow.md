# Day 40 – First GitHub Actions Workflow

Today I created my first CI/CD workflow using GitHub Actions.

## Setup

- Created new GitHub repository
- Added .github/workflows folder
- Created hello.yml workflow file

---

## Workflow Basics

- Triggered on every push
- One job named greet
- Used ubuntu-latest runner

---

## Steps Used

- actions/checkout
- Printed hello message
- Printed date and time
- Printed branch name
- Listed files
- Checked runner OS

---

## Debugging

- Added failing command intentionally
- Observed failed run
- Checked logs
- Fixed error and reran

---

## Summary

Today I learned:

- How GitHub Actions workflows run
- Workflow YAML structure
- Reading pipeline logs
- CI/CD in practical use
