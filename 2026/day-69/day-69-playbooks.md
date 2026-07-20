# Day 69 – Ansible Playbooks and Modules

## What I Learned

Today I learned how Ansible Playbooks automate server configuration using YAML.

---

# My First Playbook

Created an Ansible Playbook to:

- Install Nginx
- Start Nginx Service
- Enable Nginx on Boot
- Create a Custom Web Page

Executed the playbook successfully.

---

# Understanding Playbooks

Learned the structure of a Playbook:

- Play
- Hosts
- Tasks
- Modules

Understood how multiple tasks are grouped together to automate server configuration.

---

# Ansible Modules

Practiced commonly used modules:

- yum / apt
- service
- copy
- file
- command
- shell
- lineinfile
- debug

Each module performs a different automation task.

---

# Idempotency

Executed the same playbook twice.

Observed:

First execution:
- Packages installed
- Configuration applied

Second execution:
- No unnecessary changes
- Tasks reported **ok**

Learned that Ansible only changes the system when required.

---

# Handlers

Created a Handler to restart the Nginx service.

Observed:

- First run restarted the service after configuration changes.
- Second run did not restart because no changes were detected.

---

# Check and Diff Mode

Practiced:

- Previewing changes using Check Mode
- Viewing configuration differences using Diff Mode
- Increasing output details using Verbose mode

---

# Important Commands

```bash
ansible-playbook install-nginx.yml

ansible-playbook install-nginx.yml --check

ansible-playbook nginx-config.yml --check --diff

ansible-playbook install-nginx.yml -v

ansible-playbook install-nginx.yml --list-hosts

ansible-playbook install-nginx.yml --list-tasks
```

---

# Key Learning

Ansible Playbooks allow infrastructure automation using simple YAML files. Features like idempotency and handlers ensure configurations remain consistent, repeatable, and efficient without making unnecessary changes.
