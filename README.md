# Ansible Role: CreateVM

This Ansible role alow users to create VM under a Proxmox infrastructure.

## 1. Requirements

There is no requirements as this role will install and configure everything. You only need an ssh root access from your Ansible server to your Proxmox infrastructure.

## 2. Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml` if exist).

You **can** provide the following variables:
- OS and Version you want to deploy (Debian (10, 11 or 12), Ubuntu (24.04) or Rocky (8 or 9))
- VM Name and root password
- Hardware and network specs
- SNMP community

```yml
os2deploy: "debian"
version2deploy: 12

vm_name: "vm-test"
vm_root_pass: "azerty123#"

start_at_boot: true
memory: 4096
cores: 2
hdd_size: 50G
hdd_storage_location: local-vm

network_bridge: vmbr0
network_vlan: 301
network_ip: 192.168.4.10
network_cidr: 25
network_gw: 192.168.4.1
dns_ip: 192.168.2.134
dns_search: dewarlez.fr

snmp_community: public
```

Also, you **can** change the default locations if needed as below.

```yml
locations:
  working_folder: "/tmp"
  ansible_public_key: "~/.ssh/id_rsa.pub"
  snmp_conf: "/etc/snmp/snmpd.conf"
```

## 3. Results

You will have a VM up and running on selected network with desired parameters.

## 4. Dependencies

This role have no dependencies with other roles.

## 5. Example Playbook

```yml
- hosts: all
  vars:
    - vm_name: "vm-test"
    - vm_root_pass: "azerty123#"
    - hdd_storage_location: local-vm
    - network_ip: 192.168.4.10
  roles:
    - role: Ansible_CreateVM
  tasks:
    - debug:
        var: vm_id
```

## 6. How to test

Go to your ansible server, download this role and then browse into `roles/tests` in order to execute the following command:

```
ansible-playbook test.yml -i inventory.yml -u ansible
```

Update the inventory to match your servers (file called `inventory.yml` in tests folder) and then replace `ansible` by a user that have an access to your machine.

## 7. Todo

Nothing right now.

## 8. Author Information

This role has been writed by [Sébastien DEWARLEZ](mailto:sebastien@dewarlez.fr) for [Personal Purpose](https://blog.dewarlez.fr).