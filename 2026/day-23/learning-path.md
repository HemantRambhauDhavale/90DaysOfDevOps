# Day 23 – Git Branching & Working with GitHub

## What is a Git Branch?

A branch is a separate line of development in Git.  
It allows developers to work on features or experiments without affecting the main branch.

Branches help teams work on multiple things at the same time safely.

---

## Why We Use Branches

Instead of committing everything to the main branch, branches allow us to:

- Develop new features safely
- Test changes without breaking the main code
- Work on multiple features simultaneously

---

## What is HEAD in Git?

HEAD is a pointer that refers to the current branch and the latest commit on that branch.

When you switch branches, HEAD moves to that branch.

---

## What Happens When Switching Branches

When switching branches:

- Git changes the working directory to match the files in that branch.
- Files from other branches disappear unless merged.

---

## Git Commands Practiced

### Branch Commands

git branch  
List all branches

git branch feature-1  
Create a new branch

git switch feature-1  
Switch to branch

git checkout -b feature-2  
Create and switch branch in one command

git branch -d feature-2  
Delete branch

---

### Remote Repository

git remote add origin <repo-url>  
Connect local repo to GitHub

git push -u origin main  
Push main branch to GitHub

git push origin feature-1  
Push feature branch

git pull origin main  
Pull updates from remote repository

---

## Origin vs Upstream

Origin is the default name of the remote repository you cloned or connected.

Upstream usually refers to the original repository from which a fork was created.

---

## Fetch vs Pull

git fetch  
Downloads changes from the remote repository but does not merge them.

git pull  
Downloads changes and automatically merges them into the current branch.

---

## Clone vs Fork

Clone:
Copies a repository from GitHub to your local machine.

Fork:
Creates a copy of someone else's repository in your own GitHub account.

---

## What I Learned

1. Branches help isolate changes and prevent breaking main code.
2. GitHub allows sharing repositories and collaborating with others.
3. Understanding branching is important for real development workflows.
