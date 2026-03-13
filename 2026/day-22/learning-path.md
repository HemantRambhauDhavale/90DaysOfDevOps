# Day 22 – Introduction to Git

## What I Practiced

Today I started learning Git and created my first Git repository.

Steps I followed:

1. Installed and configured Git
2. Created a new Git repository
3. Created a git-commands.md reference file
4. Staged and committed changes
5. Built commit history with multiple commits

---

## Git Commands Used

### Setup & Config

git --version  
Check installed Git version

git config --global user.name "Hemant"

git config --global user.email "hemant@email.com"

git config --list  
Verify configuration

---

### Repository Setup

mkdir devops-git-practice

cd devops-git-practice

git init  
Initialize new Git repository

ls -a  
View .git directory

---

### Basic Workflow

git status  
Check current repository status

git add git-commands.md  
Stage file for commit

git commit -m "Initial commit"

git commit -m "Updated Git commands reference"

---

### Viewing History

git log  
View full commit history

git log --oneline  
View compact commit history

git diff  
Check file changes

---

## What I Learned

1. Git tracks file changes and maintains history.
2. Staging area allows reviewing changes before committing.
3. Git commit history helps track project evolution.
