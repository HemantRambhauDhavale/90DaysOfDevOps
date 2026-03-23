# Day 26 – GitHub CLI (gh)

Today I learned how to manage GitHub directly from the terminal using GitHub CLI.

## Installation & Authentication

- Installed GitHub CLI
- Logged in using GitHub account
- Verified active account

Authentication methods:
- Browser-based login
- Personal Access Token (PAT)

---

## Working with Repositories

- Created a new repo using gh repo create
- Cloned repo using gh repo clone
- Listed all repositories using gh repo list
- Viewed repo details
- Opened repo in browser using gh repo view --web
- Deleted a repo

---

## Issues

- Created issue using gh issue create
- Listed issues using gh issue list
- Viewed issue using gh issue view
- Closed issue using gh issue close

Use in automation:
gh issue commands can be used in scripts to automatically create or manage issues.

---

## Pull Requests

- Created branch and pushed changes
- Created PR using gh pr create
- Listed PRs using gh pr list
- Viewed PR details using gh pr view
- Merged PR using gh pr merge

Merge methods:
- Merge commit
- Squash merge
- Rebase merge

Reviewing PR:
- View PR details
- Check commits and changes
- Add comments or approve

---

## GitHub Actions (Preview)

- Listed workflows using gh workflow list
- Viewed workflow runs
- Checked workflow status

Use in CI/CD:
gh commands can help monitor pipelines and automate workflows.

---

## Useful Commands

- gh repo create
- gh repo list
- gh issue create
- gh issue list
- gh pr create
- gh pr merge
- gh workflow list
- gh run list

---

## Summary

Today I learned:

- How to use GitHub CLI
- Managing repos, issues, and PRs from terminal
- Basics of GitHub Actions using CLI
- Importance of CLI tools in DevOps automation
