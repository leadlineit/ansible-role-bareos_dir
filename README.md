# Ansible Galaxy role for install and configure Bareos Director Daemon.

![Build Status](https://github.com/leadlineit/ansible-role-bareos-dir/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.bareos_dir-blue.svg)](https://galaxy.ansible.com/leadlineit/bareos_dir/)

This role helps to install and configure Bareos Director Daemon to Debian (buster/bullseye).

Requirements
------------

This role requires Ansible 2.9 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```yaml
    bareos_dir:
      
```

The variables above are optional. They don't have a default value, so if you don't define them - tasks using them will be skipped. 
You can set only some of them, or not set at all (in this case, you will simply install Bareos Storage with default configuration). 

Variable 'bareos_release' are optional.
Default values for optional variable:

```yaml
    bareos_release: 21
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
    - hosts: servers
      roles:
         - { role: leadlineit.bareos_dir, tags: bareos_dir }
```

License
-------

MIT

Author Information
------------------

This role was created by Artem Kasianchuk.
