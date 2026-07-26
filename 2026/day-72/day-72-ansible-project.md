# Day 72 – Ansible Project: Automating Docker and Nginx Deployment

## What I Learned

Today I completed an end-to-end Ansible automation project by combining everything learned in the Ansible section.

---

# Project Architecture

Ansible Control Node

↓

Managed Server

↓

Docker Container (Port 8080)

↓

Nginx Reverse Proxy (Port 80)

↓

User Request

---

# Project Structure

- ansible.cfg
- inventory.ini
- site.yml
- group_vars/
- roles/
  - common
  - docker
  - nginx

---

# Common Role

Performed:

- Installed common packages
- Configured hostname
- Set timezone
- Created deploy user

---

# Docker Role

Automated:

- Docker installation
- Docker service
- Docker Compose
- Docker Hub Login
- Pulling Docker image
- Running Docker container

Verified:

- Docker service running
- Container running successfully

---

# Nginx Role

Configured:

- Installed Nginx
- Reverse Proxy
- Jinja2 Templates
- Nginx Configuration
- Reload Handler

Verified:

- Nginx running
- Reverse proxy working correctly

---

# Ansible Vault

Used Vault to securely store:

- Docker Hub Username
- Docker Hub Password

Benefits:

- Credentials remain encrypted
- Safer than storing passwords in plain text

---

# Jinja2 Templates

Created dynamic templates for:

- Nginx Configuration
- Reverse Proxy Configuration

Variables were automatically replaced during deployment.

---

# Tags Used

Executed:

```bash
ansible-playbook site.yml --tags docker

ansible-playbook site.yml --tags nginx

ansible-playbook site.yml --skip-tags common
