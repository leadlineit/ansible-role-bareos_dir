# Ansible Galaxy role for install and configure Bareos Director Daemon.

![Build Status](https://github.com/leadlineit/ansible-role-bareos_dir/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.bareos_dir-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/leadlineit/bareos_dir/)

This role helps to install and configure Bareos Director Daemon to Debian (buster/bullseye).

Requirements
------------

This role requires Ansible 2.9 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```yaml
---
bareos_keyserver: keyserver.ubuntu.com
bareos_apt_key: E01957D6C9FED482
bareos_release: 21
bareos_tls_path: /etc/bareos/tls
bareos_tls_certs: your.bareos.dir.com

bareos_dir:
  director: 
    - name: "{{ ansible_nodename }}"
      description: Main Bareos Director
      password: DIRAver@gEStr0ngPaSSw0rd
      tls_enable: "yes"
  bconsole:
    - name: "{{ ansible_nodename }}"
      description: Bconsole for Bareos Director
      address: localhost
      password: DIRAver@gEStr0ngPaSSw0rd
      tls_enable: "yes"
  webui:
    - name: admin_new
      password: WebUIAver@gEStr0ngPaSSw0rd1
      profile: webui-admin
    - name: user1
      password: WebUIAver@gEStr0ngPaSSw0rd2
      profile: operator
    - name: user2
      password: WebUIAver@gEStr0ngPaSSw0rd3
      profile: webui-readonly
  catalog:
    - name: MyCatalog
      description: Connect Bareos Director to PostgreSQL Database
      password: DBAver@gEStr0ngPaSSw0rd
  storage:
    - name: your-storage
      description: Your Bareos Storage
      address: ip.your.storage
      password: STORAver@gEStr0ngPaSSw0rd
      device: your-device
      tls_enable: "yes"
  pool:
    - name: your-pool
      description: Pool for backups
      volume_bytes: 100G
      volume_count: 20
      label: label-
      storage: your-storage
  fileset:
    - name: your-fileset
      description: Fileset for backups
      vss: True
      options:
        - Drive Type = fixed
        - IgnoreCase = yes
      include:
        - /path/to/files
        - /another/one/path/to/files
      plugin:
        - some plugins
      exlude:
        - .nobackup
  schedule:
    - name: your-schedule
      description: Schedule for backups
      run:
        - Full 1st sat at 21:00
        - Differential 2nd-5th sat at 21:00
        - Incremental mon-fri at 21:00
  jobdefs:
    - name: your-jobdefs
      description: Jobdefs for backups
      level: Full
      fileset: your-fileset
      schedule: your-schedule
      storage: your-storage
      pool: your-pool
  client:
    - name: your-client
      description: Your client configuration
      address: 10.0.0.1
      fdport: 9102
      passive: "yes"
      tls_enable: "yes"
      jobs:
        - name: client-job1
          description: Job1 for client
          client: client.name.com
          jobdef: your-jobdefs1
        - name: client-job2
          description: Job2 for client
          client: client.name.com
          jobdef: your-jobdefs2
  job:
    - name: client-job
      description: Job for client
      client: client.name.com
      jobdef: your-jobdefs
  messages:
    - name: your-messages
      server: your-dir
```

The variables above are optional. They don't have a default value, so if you don't define them - tasks using them will be skipped. 
You can set only some of them, or not set at all (in this case, you will simply install Bareos Storage with default configuration). 

Also you cah use HashiCorp Vault for store certificates (when you use Bareos with TLS)
Variable for this:

```yaml
---
  hashicorp_vault:
    address: your.vault.com
    token: your_token
    path: your-path-to-certs
    clients:
      - name: host1.client1
        client: client1
        ttl: 24h
      - name: host2.client1
        client: client1
        ttl: 18h
      - name: host01.client2
        client: client2
        ttl: 12h
      - name: host02.client2
        client: client2
        ttl: 96h
```

Dependencies
------------

First, for this role you need to install PostgreSQL Server. You can do it yourself, or by using my dependency role.

```yaml
---
dependencies:
  - role: leadlineit.postgresql
    tags: postgresql
```

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
