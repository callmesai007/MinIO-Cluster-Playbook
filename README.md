# MinIO Cluster Playbook

This Ansible playbook automates the deployment of a **MinIO cluster** across multiple servers. It handles everything from DNS configuration, disk setup, MinIO installation, certificate generation, user permissions, and service configuration.

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Inventory Configuration](#inventory-configuration)
4. [Playbook Structure](#playbook-structure)
5. [Usage](#usage)
6. [Configuration](#configuration)
7. [Tasks Breakdown](#tasks-breakdown)
8. [License](#license)

---

## Overview

This playbook automates the deployment of a MinIO cluster, including:

- **DNS and Hostname Configuration**: Ensures all MinIO nodes can resolve each other.
- **Disk Setup**: Formats and mounts disks for MinIO data storage.
- **MinIO Installation**: Installs MinIO on all nodes.
- **Certificate Generation**: Generates and deploys TLS certificates for secure communication.
- **User Permissions**: Configures the `minio-user` and sets appropriate permissions.
- **Service Setup**: Configures and starts the MinIO service.

---

## Prerequisites

Before using this playbook, ensure the following:

1. **Ansible**: Install Ansible on your control machine.
   ```bash
   1.1 sudo apt update
   1.2 sudo apt install ansible
   1.3 Inventory File: Define your MinIO servers in the inventory/Hosts file.
   1.4 SSH Access: Ensure Ansible can SSH into all MinIO nodes without a password (use SSH keys).
   1.5 Disk Availability: Ensure the disks you plan to use for MinIO are available on all nodes.



## Inventory Configuration
Define your MinIO servers in the inventory/Hosts file. Example:
[minio_servers]
minio-1 ansible_host=192.168.1.x
minio-2 ansible_host=192.168.1.y
minio-3 ansible_host=192.168.1.z
minio-4 ansible_host=192.168.1.t
....



## Playbook Structure
The playbook is organized as follows:

├── inventory
│   └── Hosts
├── minio-automate.yml
├── README.md
└── roles
    └── minio-automate
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   ├── certificate_generation.yml
        │   ├── disk_validation.yml
        │   ├── dns_hostname.yml
        │   ├── main.yml
        │   ├── minio_installation.yml
        │   ├── service_setup.yml
        │   └── user_permissions.yml
        ├── templates
        │   └── etc_default_minio.j2
        └── vars
            └── main.yml



## Usage
Clone the repository:

git clone https://github.com/muhamadsoufi/MinIO-Cluster-Playbook.git
cd MinIO-Cluster-Playbook
Update the inventory/Hosts file with your MinIO server details.

Run the playbook:
ansible-playbook -i inventory/Hosts minio-automate.yml


## Configuration
Default Variables
The following variables are defined in roles/minio-automate/defaults/main.yml:

minio_version: "latest"
minio_download_url: "https://dl.min.io/server/minio/release/linux-amd64/minio"
minio_install_dir: "/usr/local/bin"
minio_data_dir: "/mnt/minio"
minio_port: 9000
minio_console_port: 9001
minio_root_user: "minioadmin"
minio_root_password: "minioadmin"
minio_certs_dir: "/etc/minio/certs"
minio_opts: "--console-address :{{ minio_console_port }}"
Customizing Variables



You can override these variables in your playbook or by passing them via the command line. For example:
ansible-playbook -i inventory/Hosts minio-automate.yml -e "minio_root_password=mysecurepassword"



## Tasks Breakdown
The playbook executes the following tasks:

	DNS and Hostname Configuration:

		Adds entries to /etc/hosts for MinIO nodes.

		Sets the hostname to match the inventory name.

	Disk Validation and Setup:

		Prompts the user for the disk to use for MinIO.

		Formats the disk (if necessary) and mounts it permanent to the MinIO data directory.

	MinIO Installation:

		Downloads and installs the latest version of MinIO.

		Configures MinIO to start on boot.

	Certificate Generation:

		Generates TLS certificates for secure communication.

		Deploys certificates to all MinIO nodes.

	User Permissions:

		Creates the minio-user and sets appropriate permissions for the MinIO data and certificate directories.

	Service Setup:

		Configures the MinIO service and ensures it is running.


## License
This project is licensed under the MIT License. See the LICENSE file for details.

Author
Mohammad Hosein Soufi Ghorbani
Email: mohamadsoufi700@outlook.com
GitHub: https://github.com/muhamadsoufi

