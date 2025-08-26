# Ansible Multi-Tier Application Playbook

## Overview
This playbook deploys a secure multi-tier application stack consisting of:
- **Nginx** (web server, reverse proxy)
- **Python Flask** (application server)
- **PostgreSQL** (database server)

It also enforces OS-level password change policies and manages sensitive data securely.

## Features
- Installs and configures the latest Nginx with security best practices (IP restrictions, server_tokens off, security headers)
- Deploys a Flask application and manages it as a systemd service
- Installs and configures PostgreSQL, creates a secure database and user
- Enforces password changes every 90 days for specified users
- Sensitive variables are managed with Ansible Vault

## Usage
1. **Set up your inventory** in `inventory/hosts.yml` with your actual hostnames/IPs.
2. **Configure variables** in `group_vars/all.yml` and sensitive values in `group_vars/vault.yml`.
3. **Encrypt the vault file**:
   ```
   ansible-vault encrypt group_vars/vault.yml
   ```
4. **Run the playbook**:
   ```
   ansible-playbook -i inventory/hosts.yml app.yml
   ```
   To do a dry-run (check mode):
   ```
   ansible-playbook -i inventory/hosts.yml app.yml --check
   ```

## Security Notes
- The Nginx config restricts access by IP and hides version info.
- For production, enable HTTPS in the Nginx config (see comments in the template).
- All sensitive data (passwords) must be encrypted with Ansible Vault.

## Requirements
- Ansible 2.9+
- Target hosts: Linux (tested on Ubuntu/CentOS)

## Troubleshooting
- Ensure SSH access and sudo privileges on all target hosts.
- Check logs and debug output for failed tasks.

---

*For questions or improvements, see the playbook comments or contact the maintainer.*
