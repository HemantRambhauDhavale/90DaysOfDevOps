# Day 11 – File Ownership Challenge

## Files & Directories Created

Files:
- devops-file.txt
- team-notes.txt
- project-config.yaml
- heist-project/vault/gold.txt
- heist-project/plans/strategy.conf
- bank-heist/access-codes.txt
- bank-heist/blueprints.pdf
- bank-heist/escape-plan.txt

Directories:
- app-logs/
- heist-project/
- bank-heist/

---

## Ownership Changes

Example changes:

- devops-file.txt: user:user → tokyo:user
- team-notes.txt: user:user → user:heist-team
- project-config.yaml → professor:heist-team
- app-logs/ → berlin:heist-team
- heist-project/ → professor:planners (recursive)
- access-codes.txt → tokyo:vault-team
- blueprints.pdf → berlin:tech-team
- escape-plan.txt → nairobi:vault-team

---

## Commands Used

# View ownership
ls -l

# Change owner
sudo chown tokyo devops-file.txt
sudo chown berlin devops-file.txt

# Change group
sudo groupadd heist-team
sudo chgrp heist-team team-notes.txt

# Change owner and group together
sudo chown professor:heist-team project-config.yaml

# Recursive change
sudo chown -R professor:planners heist-project/

# Practice challenge
sudo chown tokyo:vault-team bank-heist/access-codes.txt
sudo chown berlin:tech-team bank-heist/blueprints.pdf
sudo chown nairobi:vault-team bank-heist/escape-plan.txt

---

## What I Learned

- Difference between file owner and file group
- How chown and chgrp work
- Importance of recursive ownership
- Ownership directly impacts application access
