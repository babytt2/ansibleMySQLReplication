# Ansible Role: MySQL Replication Setup

This Ansible role installs MySQL and configures replication automatically on both on-premises and cloud-based EC2 instances.

## Requirements

- Ansible 2.9+
- EC2 instances with SSH access
- MySQL repository access

# inventory
[master]
10.10.10.111 #masterip

[replica]
10.10.10.112 #replicaip


# Example Playbook
- hosts: master
  become: yes
  gather_facts: yes
  vars:
    database_engine: mysql # mysql, mariadb
    version: 8.0 # 10.4, 5.7, 8.0
    set_environment: prod
    when: "'master' in group_names"
  roles:
    - replication

- hosts: replica
  become: yes
  gather_facts: yes
  vars:
    database_engine: mysql # mysql. mariadb
    version: 8.0
    set_environment: prod
    always_reset_replication: true
    when: "'replica' in group_names"
  roles:
    - replication
