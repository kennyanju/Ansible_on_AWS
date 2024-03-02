
# Ansible for Amazon EC2: A Comprehensive Guide

## 1. Introduction to Ansible and Amazon EC2

Ansible is an open-source automation tool for software provisioning, configuration management, and application deployment. Amazon EC2 provides scalable computing capacity in the Amazon Web Services (AWS) cloud, making it easier for developers to run applications in the cloud.

## 2. Installing Ansible

### Prerequisites

- Python (version 2.7 or later, or 3.5 or later)
- Pip (Python package manager)

### Installation Commands

```bash
sudo apt update
sudo apt install python3-pip -y
pip3 install ansible
```

## 3. Using Ansible for Orchestration

### Basic Orchestration Example

```yaml
- hosts: all
  tasks:
    - name: Ensure Apache is installed
      yum:
        name: httpd
        state: present
```

## 4. Using Host Groups

### Defining Host Groups in the Inventory

```ini
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
```

## 5. Using Tags

### Tagging Tasks and Running Them Selectively

```yaml
tasks:
  - name: Install Apache
    yum:
      name: httpd
      state: present
    tags: 
      - installation
```

## 6. Running Tasks Against Localhost

### Configuring Ansible to Run Tasks on Localhost

```yaml
- hosts: localhost
  connection: local
  tasks:
    - name: Check disk usage
      command: df -h
```

## 7. Using the Command Line to Control Execution

### Command Line Options for Running Ansible

```bash
ansible-playbook playbook.yml --tags "installation"
ansible-playbook playbook.yml --limit webservers
```

## 8. Specifying Variables in the Inventory File

### How to Define and Use Variables

```ini
[webservers]
web1.example.com http_port=80
web2.example.com http_port=8080
```

## 9. Creating Dynamic Inventory Files

### Generating and Using Dynamic Inventories

Dynamic inventories can be generated from various sources. This section would involve a script example for AWS EC2.

## 10. Using Templates

### Template Basics and Examples

```jinja
Welcome to {{ ansible_hostname }}
```

## 11. Conditional Execution

### Writing Tasks with Conditions

```yaml
- name: "Restart webserver"
  service:
    name: httpd
    state: restarted
  when: ansible_os_family == "RedHat"
```

## 12. Using Loops

### Looping Through Tasks in Ansible

```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - tree
```

# Advanced Ansible Guide: Mastering Automation on Amazon EC2


## Testing Plays with Check Mode

Check mode (or "dry run") allows you to test plays without making any real changes to your servers.

### Example Command

```bash
ansible-playbook playbook.yml --check
```

## Using Variable Roles

Variable roles enable you to dynamically include roles based on variables.

### Defining Variable Roles

```yaml
- hosts: all
  roles:
    - role: "{{ user_specified_role }}"
```

## Using Role-Based Templates

Role-based templates help in separating variables and files based on roles.

### Example Structure

```
roles/
  common/
    templates/
      ntp.conf.j2
```

## Role Management

Roles are a way to organize playbooks and allow for reusable components.

### Creating a Role

```bash
ansible-galaxy init my_new_role
```

## Using Ansible Vault

Ansible Vault is used to encrypt sensitive data.

### Encrypting Files

```bash
ansible-vault encrypt secret.yml
```

## Using Secrets in Plays

Incorporate secrets securely in your plays using Ansible Vault.

### Example Usage

```yaml
- hosts: all
  vars_files:
    - secrets.yml
```

## Working with IP Addresses

Ansible provides filters and modules to work with IP addresses.

### Example Task

```yaml
- name: Validate IP Address
  fail:
    msg: "Invalid IP"
  when: not (ip_addr | ipaddr('ipv4'))
```

## Working with Network Devices

Ansible can manage network devices using specific modules.

### Example Network Play

```yaml
- hosts: switches
  gather_facts: no
  tasks:
    - name: Ensure VLAN exists
      ios_vlan:
        vlan_id: 100
        name: "Data_VLAN"
        state: present
```

## Registering Discovered State

Use the `register` keyword to capture the output of a task and use it in subsequent tasks.

### Example Registration

```yaml
- name: Check disk space
  command: df -h
  register: disk_space
```