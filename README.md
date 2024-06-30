# Ansible Role: CreateVM

This Ansible role alow users to create a VM under a Proxmox infrastructure.

## 1. Requirements

There is no requirements as this role will install and configure everything. You only need an ssh root access from your Ansible server to your Proxmox infrastructure.

## 2. Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml` if exist).

You **can** provide the following variables:
- OS and Version you want to deploy (Debian (10, 11 or 12), Ubuntu (24.04) or Rocky (8 or 9))
- VM Name and root password
- Hardware and network specs

```yml
host_config:
  # Choose OS version you want to deploy (Debian (10, 11 or 12), Ubuntu (24.04) or Rocky (8 or 9))
  os: debian
  version: 12

  # Enter hostname and domain name for your VM
  hostname: vm-name
  domain: example.com

  # Provide root default password
  root_password: Ch@ngeMe
  
  # Hardware specs
  hw:
    start_at_boot: true
    memory: 4096
    cores: 2
    hdd_size: 50G
    hdd_storage_location: local-vm

  # Network specs
  network:
    bridge: vmbr0
    vlan: 301
    ip: 192.168.4.10
    cidr: 25
    gw: 192.168.4.1
    dns_ip: 192.168.2.134
```

Also, you **can** change the default locations if needed as below.

```yml
# File and folder default location
locations:
  working_folder: "/tmp"
  ansible_public_key: "~/.ssh/id_rsa.pub"
```

Or add your custom Cloud-Init images.
```yml
# Cloud-Init image location
qcow2_images:
  debian:
    12: "https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-genericcloud-amd64.qcow2"
    11: "https://cloud.debian.org/images/cloud/bullseye/latest/debian-11-genericcloud-amd64.qcow2"
    10: "https://cloud.debian.org/images/cloud/buster/latest/debian-10-genericcloud-amd64.qcow2"
  ubuntu:
    24.04: "http://cloud-images.ubuntu.com/releases/noble/release/ubuntu-24.04-server-cloudimg-amd64.img"
  rocky:
    9: "http://dl.rockylinux.org/pub/rocky/9/images/x86_64/Rocky-9-GenericCloud-Base.latest.x86_64.qcow2"
    8: "http://dl.rockylinux.org/pub/rocky/8/images/x86_64/Rocky-8-GenericCloud-Base.latest.x86_64.qcow2"
```

## 3. Results

You will have a VM up and running on selected network with desired parameters.

## 4. Dependencies

This role have no dependencies with other roles.

## 5. How to test and examples

Go to your ansible server, download this role and then browse into `roles/tests` in order to execute the following command:

```
ansible-playbook test.yml -i inventory.yml -u ansible
```

Update the inventory to match your servers (file called `inventory.yml` in tests folder) and then replace `ansible` by a user that have an access to your machine.

## 6. Todo

Nothing right now.

## 7. Author Information

This role has been writed by [Sébastien DEWARLEZ](mailto:sebastien@dewarlez.fr) for [Personal Purpose](https://blog.dewarlez.fr).