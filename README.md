# Ansible Role: CreateVM

This Ansible role alow users to create and configure a VM under a Proxmox infrastructure.

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

# SNMP configuration
snmp_config:
  configure: yes # Can be yes or no (bolean)
  rocommunity: public
  syscontact: sebastien@dewarlez.fr
  syslocation: home
  conf_location: /etc/snmp/snmpd.conf
  service_name: snmpd
  backup_config: yes # Can be yes or no (bolean)

# NTP client configuration
ntp_config:
  configure: yes # Can be yes or no (bolean)
  server: 192.168.2.134
  timezone: Europe/Paris
  conf_location:
    debian: /etc/ntpsec/ntp.conf
    ubuntu: /etc/ntpsec/ntp.conf
    rocky: /etc/ntp.conf
  service_name:
    debian: ntpd
    ubuntu: ntpd
    rocky: ntpd
  backup_config: yes # Can be yes or no (bolean)

# Syslog configuration
syslog_config:
  configure: yes # Can be yes or no (bolean)
  server: syslog.dewarlez.fr
  conf_location: /etc/rsyslog.d/custom.conf
  service_name: rsyslog
  backup_config: yes # Can be yes or no (bolean)

# SMTP configuration to allow server sending email
smtp_config:
  configure: yes # Can be yes or no (bolean)
  host: smtp.dewarlez.fr
  port: 25
  test: yes # Can be yes or no (bolean)
  test_from: noreply@dewarlez.fr
  test_to: sebastien@dewarlez.fr
  conf_location: /etc/mail.rc
  backup_config: yes # Can be yes or no (bolean)

# QEMU Agent configuration
qemu_agent_config:
  configure: yes # Can be yes or no (bolean)
  service_name: qemu-guest-agent
  backup_config: yes # Can be yes or no (bolean)

# Bashrc customisation
bash_config:
  configure: yes # Can be yes or no (bolean)
  default_bashrc: /etc/profile.d/default_bashrc.sh
  root_bashrc: /root/.bashrc
  backup_config: yes # Can be yes or no (bolean)
  alert_login:
    enable: no # Can be yes or no (bolean)
    send_to: sebastien@dewarlez.fr
    send_from: noreply@dewarlez.fr

# Create a series of default users
users_config:
  configure: yes # Can be yes or no (bolean)
  users:
    - name: root
      password: "$6$cDP4/kfEjABOFzju$absFqDNB2KVrcxGQTRZkBbFuK5CAIMXUUPqu2/HSkYRSfeUZwyN5XjmUxApYuYCiHHDnjhvOBVzDjmrRXrsk6." # poiuyt123$
      state: present # Can be present or absent
      createhome: no # Can be yes or no (bolean)
      sudo: yes # Can be yes or no (bolean)
    - name: sdewarle
      password: "$6$cDP4/kfEjABOFzju$absFqDNB2KVrcxGQTRZkBbFuK5CAIMXUUPqu2/HSkYRSfeUZwyN5XjmUxApYuYCiHHDnjhvOBVzDjmrRXrsk6." # poiuyt123$
      state: present # Can be present or absent
      createhome: yes # Can be yes or no (bolean)
      sudo: yes # Can be yes or no (bolean)

# Parameters to reconfigure SSH service
security_config:
  configure: yes # Can be yes or no (bolean)
  conf_location: /etc/ssh/sshd_config
  service_name: sshd
  backup_config: yes # Can be yes or no (bolean)

# File and folder default location
locations:
  working_folder: "/tmp"
  ansible_public_key: "~/.ssh/id_rsa.pub"

# Python is needed for ansible to work correctly, please provide package name and path
python:
  pkg_name: python3
  path: /usr/bin/python3

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