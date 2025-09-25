# cloudsible
# Ansible Multi-Server Provisioning Playbook

This repository contains an Ansible playbook for automating the setup of web and database servers on Fedora and Ubuntu systems running on AWS EC2 instances. It controls 4 EC2 instances (2 web servers and 2 DB servers), demonstrating cloud infrastructure management skills such as provisioning virtual machines, secure remote automation, and environment separation (production vs. staging). The playbook handles OS-specific package updates, base configurations, and role-specific installations (e.g., Apache/PHP for web servers with custom configs and handlers, MariaDB for DB servers). Ideal for learning Ansible, DevOps practices, cloud orchestration, or bootstrapping a simple scalable infrastructure on AWS.

## Features
- **Cross-Distro Support**: Conditional tasks for Fedora (DNF) and Ubuntu (APT) package management.
- **Modular Roles**: Separates concerns with roles for base setup, web servers, and DB servers.
- **Variable Overrides**: Uses group_vars files (e.g., `db_servers.yml`, `web_servers.yml`) for customizable packages and services.
- **Inventory Management**: Points to a production inventory file for targeting hosts.
- **Cloud Integration**: Designed for AWS EC2, showcasing skills in instance provisioning, security groups for SSH, private key authentication, and remote execution.   Assumes instances are launched with appropriate AMIs (Ubuntu/Fedora) and IAM Roles.
- **Handlers and Notifications**: Includes handlers for service restarts (e.g., Apache) triggered by config changes.
- **Static Site Deployment**: Deploys a sample  web application (front-end layout for a restaurant called "Little Lemon") from GitHub to the web servers' document root (/var/www/html) on Ubuntu, using Ansible's git module for easy updates.

## Prerequisites
- Ansible 2.10+ installed (tested on 2.16).
- Python 3.12 on control node (as specified in `ansible.cfg`).
- Inventory file at `./inventories/production/hosts.ini` with groups like `[web_servers]` and `[db_servers]`.
- AWS EC2 instances or Virtual Machines on local host: 4 instances total (e.g., 2 Ubuntu/Fedora for web servers, 2 for DB servers).
- Instances are running and accessible via SSH.
- Security groups allow inbound SSH (port 22) from your control node's IP.

## Project Structure
```
.
├── ansible.cfg                  # Ansible config: interpreter, inventory, roles path, etc.
├── inventories/
│   ├── production/              # Production environment
│   │   ├── group_vars/
│   │   │   ├── db_servers.yml   # DB vars (e.g., MariaDB package)
│   │   │   └── web_servers.yml  # Web vars (e.g., Apache/PHP packages)
│   │   └── hosts.ini            # Production hosts/groups
│   └── staging/                 # Staging environment
│       ├── group_vars/
│       │   ├── db_servers.yml   # Staging DB vars
│       │   └── web_servers.yml  # Staging web vars
│       └── hosts.ini            # Staging hosts/groups
├── LICENSE                      # MIT License
├── playbooks/
│   ├── install_apache.yml       # Targeted playbook for Apache/PHP on Ubuntu
│   └── site.yml                 # Main playbook: updates, base role, web/DB roles
├── README.md                    # This file
└── roles/
    ├── base/                    # Base role: common setup
    │   ├── files/
    │   │   └── sudoer_simone    # Custom sudoers file
    │   └── tasks/
    │       └── main.yml         # Base tasks (e.g., install common tools)
    ├── db_servers/              # DB role: MariaDB installation
    │   └── tasks/
    │       └── main.yml         # DB tasks
    └── web_servers/             # Web role: Apache/PHP setup
        ├── files/
        │   └── deploy_site.yml  # Task file for deploying static site from GitHub (e.g., clones https://github.com/mahmoudnasser1561/Little_lemon to /var/www/html on Ubuntu)
        ├── handlers/
        │   └── main.yml         # Handlers (e.g., restart Apache)
        └── tasks/
            └── main.yml         # Web tasks: install Apache/PHP, config edits (e.g., ServerAdmin email), service start, and site deployment
```

## Expected Output
On a successful run, all 4 EC2 instances will receive OS updates, base configs (e.g., sudoers), web servers will have Apache/PHP installed with custom admin email, running services, and a deployed static site (Little Lemon restaurant app front-end) served from /var/www/html. DB servers will have MariaDB set up. Handlers ensure services restart on config changes.
