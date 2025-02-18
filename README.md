# Ansible Playbook for Docker Installation and User Configuration

## Overview
This Ansible playbook automates the installation and configuration of Docker on multiple Linux servers. It:
- Installs required dependencies.
- Adds Docker's official GPG key.
- Adds the Docker repository.
- Installs Docker and necessary plugins.
- Ensures the user is added to the Docker group for sudo-less Docker access.
- Restarts the Docker service and verifies the installation.

## Prerequisites
1. Ensure you have Ansible installed on your control machine:
   ```sh
   sudo apt update && sudo apt install ansible -y
   ```
2. Ensure SSH access is configured between the control machine and target servers.
3. Generate an SSH key pair (if not already generated):
   ```sh
   ssh-keygen -t rsa -b 4096
   ```
4. Copy the public key to the target servers:
   ```sh
   ssh-copy-id username@server_ip
   ```
5. Update your `inventory.ini` file with correct host details and SSH private key.

## Inventory File Example (`inventory.ini`)
```ini
[myservers]
server1 ansible_host=your_server_ip ansible_user=username ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_become_pass=password
server2 ansible_host=your_server_ip ansible_user=username ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_become_pass=password
server3 ansible_host=your_server_ip ansible_user=username ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_become_pass=password
```

## Running the Playbook
To execute the playbook, run:
```sh
ansible-playbook -i inventory.ini docker_setup.yml
```

## Playbook Breakdown
This playbook performs the following tasks:
1. **Checks if Docker is installed**
2. **Uninstalls conflicting packages** (if necessary)
3. **Installs required dependencies**
4. **Adds Docker's official GPG key and repository**
5. **Updates package index and installs Docker Engine**
6. **Adds the user to the Docker group for password-less execution**
7. **Restarts Docker service and verifies installation**

## Verifying Docker Installation
After running the playbook, verify Docker installation by SSH-ing into the target servers and running:
```sh
docker --version
```
To check if the user has been added to the `docker` group:
```sh
groups username
```

## Troubleshooting
### SSH Issues
- Ensure SSH keys are correctly configured using `ssh-copy-id`.
- Verify connectivity using:
  ```sh
  ansible all -i inventory.ini -m ping
  ```

### Docker Installation Issues
- Check if Docker is installed:
  ```sh
  dpkg -l | grep docker
  ```
- If `apt` fails due to broken dependencies, run:
  ```sh
  sudo apt --fix-broken install
  ```

### User Group Issues
- If the user is not in the `docker` group, add manually:
  ```sh
  sudo usermod -aG docker username
  ```
- Logout and re-login for changes to take effect.

## Notes
- This playbook is tested on Debian-based distributions (Kali Linux, Ubuntu, Debian).
- If running on a fresh install, ensure all dependencies are updated with:
  ```sh
  sudo apt update && sudo apt upgrade -y
  ```
- Modify the `inventory.ini` file based on your server environment.

## Conclusion
This Ansible playbook simplifies Docker installation and user configuration across multiple servers, ensuring a seamless setup for containerized workloads. 🚀

