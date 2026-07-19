# Day 68 – Introduction to Ansible and Inventory Setup

## What I Learned

Today I learned the basics of Ansible and how it helps automate server configuration and management.

---

# Understanding Ansible

Learned:

- What is Configuration Management
- Why automation is important
- Difference between Ansible and other configuration management tools
- Agentless Architecture

---

# Ansible Architecture

Components learned:

- Control Node
- Managed Nodes
- Inventory
- Modules
- Playbooks

Understood how Ansible connects to remote servers using SSH without installing agents.

---

# Installing Ansible

Installed Ansible on the Control Node.

Verified installation using:

ansible --version

---

# Inventory File

Created an inventory file containing:

- Web Server
- App Server
- Database Server

Grouped servers for easier management.

---

# Ad-Hoc Commands

Executed commands using different Ansible modules.

Practiced:

- Ping remote servers
- Check uptime
- View memory usage
- Check disk usage
- Install packages
- Copy files

---

# Inventory Groups

Created groups for:

- Web
- App
- Database

Also learned group combinations using:

- application
- all_servers

Practiced host patterns to target specific groups.

---

# Important Commands

ansible --version

ansible all -m ping

ansible all -m command -a "uptime"

ansible all -m command -a "df -h"

ansible web -m yum -a "name=git state=present" --become

ansible all -m copy -a "src=file dest=/tmp/file"

---

# Key Learning

Ansible is an agentless configuration management tool that uses SSH to automate administrative tasks across multiple servers from a single control node, making infrastructure management faster, simpler, and more consistent.
