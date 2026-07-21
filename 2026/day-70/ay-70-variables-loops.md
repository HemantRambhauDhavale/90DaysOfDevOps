# Day 70 – Variables, Facts, Conditionals and Loops

## What I Learned

Today I learned how to build dynamic Ansible Playbooks using variables, facts, conditionals, and loops.

---

# Variables

Created variables inside playbooks for:

- Application Name
- Port Number
- Directory Path
- Package List

Learned how variables make playbooks reusable.

---

# group_vars and host_vars

Organized variables using:

- group_vars/all.yml
- group_vars/web.yml
- group_vars/db.yml
- host_vars/web-server.yml

Learned how different variables apply to different hosts.

---

# Ansible Facts

Collected system information automatically.

Practiced retrieving:

- Hostname
- Operating System
- IP Address
- Memory
- Network Interfaces

Used facts inside playbooks.

---

# Conditionals

Used **when** conditions to:

- Install Nginx only on web servers
- Install MySQL only on database servers
- Run tasks based on operating system
- Execute tasks only for specific environments

---

# Loops

Automated repetitive tasks using loops.

Created:

- Multiple users
- Multiple directories
- Multiple package installations

Reduced duplicate code inside playbooks.

---

# Register and Debug

Stored command output using **register**.

Displayed results using the **debug** module.

Created a server report using collected information.

---

# Important Commands

```bash
ansible-playbook variables-demo.yml

ansible-playbook variables-demo.yml -e "app_name=my-custom-app app_port=9090"

ansible web-server -m setup

ansible web-server -m setup -a "filter=ansible_distribution*"

ansible-playbook conditional-demo.yml

ansible-playbook loops-demo.yml

ansible-playbook server-report.yml
```

---

# Key Learning

Ansible becomes much more flexible when variables, facts, conditionals, and loops are combined. A single playbook can automatically adapt to different servers, operating systems, and environments, making configuration management scalable and efficient.
