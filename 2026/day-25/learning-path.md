# Day 25 – Git Reset vs Revert & Branching Strategies

Today I focused on understanding how Git handles mistakes and how teams manage branches in real-world development.

## Git Reset – Hands-On

I created three commits in my practice repository.

Commit A  
Commit B  
Commit C  

Then I tested different reset options.

### git reset --soft

This moved the HEAD back to the previous commit but kept the changes staged.

Changes were still ready to commit.

### git reset --mixed

This moved the HEAD back but unstaged the changes.

The changes stayed in the working directory but were no longer staged.

### git reset --hard

This removed the commit and also deleted the changes from the working directory.

This option is destructive because changes cannot be easily recovered unless they exist in reflog.

### Key Difference

| Option | What Happens |
|------|------|
| --soft | Commit removed but changes stay staged |
| --mixed | Commit removed and changes become unstaged |
| --hard | Commit removed and changes deleted |

### When to Use

--soft → when you want to modify a commit  
--mixed → when you want to redo staging  
--hard → when you want to completely discard changes

Important rule: avoid using reset on commits that are already pushed to shared repositories.

---

## Git Revert – Hands-On

I created three commits again:

Commit X  
Commit Y  
Commit Z  

Then I reverted commit Y.

Git created a new commit that reversed the changes made by commit Y.

### Observation

Commit Y still existed in history, but its effect was cancelled.

This is why revert is considered safer.

### Reset vs Revert

| Feature | git reset | git revert |
|------|------|------|
| Changes history | Yes | No |
| Removes commits | Yes | No |
| Safe for shared branches | No | Yes |
| Usage | Local undo | Undo in shared repos |

---

## Branching Strategies

### GitFlow

A structured branching model.

Main branches:
- main
- develop

Other branches:
- feature
- release
- hotfix

Used by teams with scheduled releases.

Pros:
- organized workflow
- clear release management

Cons:
- complex for small teams.

---

### GitHub Flow

Simple workflow used in many open-source projects.

Steps:

main → create feature branch → commit changes → open pull request → merge.

Pros:
- simple
- easy collaboration.

Cons:
- less structured for large release cycles.

---

### Trunk-Based Development

Developers commit frequently to a single main branch.

Feature branches exist but are short-lived.

Pros:
- fast integration
- reduces merge conflicts.

Cons:
- requires strong testing and CI/CD.

---

## Strategy Choice

For startups shipping fast → GitHub Flow or Trunk-Based Development.

For large teams with scheduled releases → GitFlow.

---

## Important Command Discovered

git reflog

It tracks every change to HEAD and helps recover lost commits after reset or other mistakes.

---

## Summary

Today helped me understand:

- Different ways to undo commits
- Safe vs unsafe Git operations
- How engineering teams manage branches
- Why reflog is an important safety tool
