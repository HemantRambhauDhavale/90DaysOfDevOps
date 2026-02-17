# Day 09 – Linux User & Group Management Challenge

## Users & Groups Created

Users:
- tokyo
- berlin
- professor
- nairobi

Groups:
- developers
- admins
- project-team

---

## Commands Used

### Create Users
sudo useradd -m tokyo
sudo useradd -m berlin
sudo useradd -m professor
sudo useradd -m nairobi

sudo passwd tokyo
sudo passwd berlin
sudo passwd professor
sudo passwd nairobi

---

### Create Groups
sudo groupadd developers
sudo groupadd admins
sudo groupadd project-team

---

### Assign Users to Groups
sudo usermod -aG developers tokyo
sudo usermod -aG developers,admins berlin
sudo usermod -aG admins professor
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo

Verify:
groups tokyo
groups berlin
groups professor
groups nairobi

---

## Shared Directory Setup

### Create Directory
sudo mkdir -p /opt/dev-project
sudo chgrp developers /opt/dev-project
sudo chmod 775 /opt/dev-project

Test:
sudo -u tokyo touch /opt/dev-project/test1.txt
sudo -u berlin touch /opt/dev-project/test2.txt

---

## Team Workspace Setup

sudo mkdir -p /opt/team-workspace
sudo chgrp project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace

Test:
sudo -u nairobi touch /opt/team-workspace/teamfile.txt

---

## What I Learned

- Difference between primary and secondary groups
- Importance of 775 permissions in shared environments
- How group ownership controls access
- Real team-based permission setup in Linux
