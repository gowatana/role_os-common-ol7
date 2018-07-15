Oracle Linux 7 Common settings.
===============================

Ansible role for home lab.

Requirements
------------

OS template setting.

```
$ TARGET_TEMP_IP=192.168.1.99
$ DGW=192.168.1.1

$ ssh-copy-id root@${TARGET_TEMP_IP}
$ ssh root@${TARGET_TEMP_IP} "nmcli c mod ens192 ipv4.gateway ${DGW}"
$ ssh root@${TARGET_TEMP_IP} "nmcli c mod ens192 ipv4.dns 192.168.1.101,192.168.1.102"
$ ssh root@${TARGET_TEMP_IP} "nmcli c mod ens192 ipv4.dns-search go-lab.jp"
```

Before ansible-playbook

```
$ TARGET_TEMP_IP=192.168.1.99
$ TARGET_IP=192.168.1.100
$ PREFIX=24

$ ssh root@${TARGET_TEMP_IP} "nmcli c mod ens192 ipv4.addresses ${TARGET_IP}/${PREFIX}"
$ ssh root@${TARGET_TEMP_IP} "nmcli c mod ens192 ipv4.method manual"
$ ssh root@${TARGET_TEMP_IP} "reboot"
$ ssh root@${TARGET_IP} "uptime"
```

Role Variables
--------------

./vars/main.yml only.


Dependencies
------------

no dependencies.


Example Playbook
----------------

    - hosts: servers
      roles:
         - gowatana.os-common-ol7


## Ansible Galaxy role.

Galaxy install

```
$ cd playbook
$ ansible-galaxy install -p ./roles gowatana.os-common-ol7
$ ansible-galaxy install --force -p ./roles gowatana.os-common-ol7
```

add vrars ./group_vars/all file.

```
lab_gateway: 192.168.1.1
lab_domain: go-lab.jp
lab_dns_1: 192.168.1.101
lab_dns_2: 192.168.1.102

```

## Ansible Playbook.

```
$ git clone https://github.com/gowatana/role_os-common-ol7.git
$ mv role_os-common-ol7 playbook/os-common-ol7
$ cd playbook/os-common-ol7/

$ echo "host_short_name ansible_host=${TARGET_IP}" >> hosts
$ ansible -i hosts --list-hosts all

$ vi roles/os-common-ol7/vars/main.yml
$ vi files/etc/hosts

$ ansible-playbook -i hosts setup-ol7-os.yml -C
$ ansible-playbook -i hosts setup-ol7-os.yml
```

## OS Reboot

```
$ ansible -i hosts -u root -a "uptime" all
$ ansible -i hosts -u root -a "reboot" all
$ ansible -i hosts -u root -a "uptime" all
```

License
-------

BSD

