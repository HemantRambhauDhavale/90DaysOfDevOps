# Day 71 – Ansible Roles, Templates, Galaxy and Vault

## What I Learned

Today I learned how to organize large Ansible projects using Roles, create dynamic configuration files with Jinja2 Templates, reuse community automation through Ansible Galaxy, and securely manage secrets using Ansible Vault.

---

# Jinja2 Templates

Created dynamic templates for:

- Nginx Virtual Host
- Index Page
- Database Configuration

Learned how variables and Ansible Facts automatically generate different configuration files for different servers.

---

# Ansible Roles

Created my first custom **webserver** role.

Role structure included:

- tasks/
- handlers/
- templates/
- defaults/
- vars/
- files/
- meta/

Moved Nginx installation and configuration into the role.

---

# Ansible Galaxy

Practiced:

- Searching Galaxy roles
- Installing Docker role
- Managing roles using requirements.yml

Learned how community roles save time by providing reusable automation.

---

# Ansible Vault

Created an encrypted vault file.

Stored:

- Database Password
- Root Password
- API Key

Practiced:

- Create
- Edit
- View
- Encrypt
- Use encrypted variables in playbooks

---

# Combined Everything

Created a complete playbook using:

- Custom Webserver Role
- Galaxy Docker Role
- Jinja2 Templates
- Vault Secrets

Verified dynamic configuration files and encrypted secrets were applied correctly.

---

# Important Commands

```bash
ansible-playbook template-demo.yml --diff

ansible-galaxy init roles/webserver

ansible-playbook site.yml

ansible-galaxy search nginx

ansible-galaxy install geerlingguy.docker

ansible-galaxy install -r requirements.yml

ansible-vault create group_vars/db/vault.yml

ansible-vault edit group_vars/db/vault.yml

ansible-vault view group_vars/db/vault.yml

ansible-playbook db-setup.yml --ask-vault-pass
```

---

# Key Learning

Roles make Ansible automation modular and reusable. Jinja2 Templates generate dynamic configuration files, Galaxy provides reusable community roles, and Vault secures sensitive information. Together they create a production-ready automation workflow.
