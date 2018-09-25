# CentOS 7 Common Initial Setup Tasks

This role will dramatically reconfigure log management and rotation.

The goal of this role is to alter and improve how system logs are rotated by using 
a high compression (max setting for xz), daily rotation, and short retention (30 days).

## Requirements

None.

## Role Variables

None.

## Dependencies

None.

## Example Playbook

Fetch this role from Ansible Galaxy:

`ansible-galaxy install mariuszczyz.centos-logrotation-on-steroids`

In playbook.yml:

```bash
- hosts: servers
  roles:
    - { role: mariuszczyz.centos-logrotation-on-steroidss, tags: ['logrotation-on-steroids'] }
```

Before any SSH hardening configuration changes are applied this ansible-playbook command should be ran (as root):

`ansible-playbook -i hosts playbook.yml --user root --ask-pass --limit=hostname`

After SSH hardening configuration changes are made which prohibit root login run ansible-playbook as regular user:

`ansible-playbook -i hosts playbook.yml --user username --become --ask-sudo-pass --limit=hostname`

## License

BSD

## Author Information

Author: Mariusz Czyz  

Date: 09/2018
