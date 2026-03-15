# Day 24 – Advanced Git Concepts

## Git Merge

Git merge combines changes from one branch into another.

Example workflow:

git switch main  
git merge feature-login  

### Fast Forward Merge

A fast-forward merge happens when the main branch has no new commits and Git simply moves the pointer forward.

### Merge Commit

When both branches have new commits, Git creates a merge commit to combine histories.

### Merge Conflict

A merge conflict occurs when two branches modify the same line of a file and Git cannot automatically decide which change to keep.

---

## Git Rebase

Rebase moves a branch to start from a new base commit.

Example:

git switch feature-dashboard  
git rebase main  

This rewrites the commit history by replaying commits on top of the latest main branch.

### Difference Between Merge and Rebase

Merge keeps the original branch history.

Rebase rewrites history to make it appear linear.

Rebasing commits that are already shared with others should be avoided.

---

## Squash Merge

Squash merge combines multiple commits into a single commit before merging.

Example:

git merge --squash feature-profile  

This keeps the main branch history clean.

---

## Git Stash

Git stash temporarily saves changes that are not committed.

Example:

git stash  
git stash list  
git stash pop  

### stash pop vs stash apply

git stash pop applies the stash and removes it from the stash list.

git stash apply applies the stash but keeps it in the stash list.

---

## Git Cherry Pick

Cherry pick allows applying a specific commit from another branch.

Example:

git cherry-pick <commit-hash>

This is useful when you want to bring only one fix instead of merging the entire branch.

---

## What I Learned

1. Git merge and rebase are different ways to combine branch histories.
2. Git stash is useful when switching tasks quickly.
3. Cherry-pick helps apply specific fixes without merging the entire branch.
