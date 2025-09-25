# cloudsible
# Ansible Multi-Server Provisioning Playbook

This repository contains an Ansible playbook for automating the setup of web and database servers on Fedora and Ubuntu systems. It handles package updates, base configurations, and role-specific installations (e.g., Apache/PHP for web servers, MariaDB for DB servers). Ideal for learning Ansible or bootstrapping simple infrastructure.

## Features
- **Cross-Distro Support**: Conditional tasks for Fedora (DNF) and Ubuntu (APT) package management.
- **Modular Roles**: Separates concerns with roles for base setup, web servers, and DB servers.
- **Variable Overrides**: Uses group_vars files (e.g., `db_servers.yml`, `web_servers.yml`) for customizable packages and services.
- **Inventory Management**: Points to a production inventory file for targeting hosts.

## Prerequisites
- Ansible 2.10+ installed (tested on 2.16).
- Python 3.12 on control node (as specified in `ansible.cfg`).
- Inventory file at `./inventories/production/hosts.ini` with groups like `[web_servers]` and `[db_servers]`.
