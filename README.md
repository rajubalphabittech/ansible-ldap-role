## Overview

This is an "*ansible role*" to manage [OpenLDAP](https://ltb-project.org/doku.php): installation and configuration.

I tried to keep it as simple as I can

~~~
[ansible-ldap-role]$ tree
.
├── defaults
│   └── main.yml
├── files
│   └── schema
│       ├── eduPerson.schema
│       ├── krb5-kdc.schema
│       ├── samba.schema
│       └── schac.schema
├── handlers
│   └── main.yml
├── README.md
├── tasks
│   ├── debian_ldap.yml
│   ├── main.yml
│   ├── redhat_ldap.yml
│   └── users_ldap.yml
├── templates
│   ├── reader.ldif
│   ├── slapd.conf
│   ├── slapd-dump
│   ├── slapd-init
│   ├── slapdlocal.conf
│   └── startup.ldif
└── vars
    └── main.yml
~~~

## Tested on

Debian/Ubuntu

## Usage

You can try it with a playbook like this

~~~
$> more devel.yml
---
- name: Develop a new Role
  hosts: test1
  vars:
    proj_name: ldap
    proj_path: "/opt/{{ proj_name }}"
  tasks:
    - name: install apt packages
      apt: name={{ item }} update_cache=yes cache_valid_time=3600
      become: true
      with_items:
        - curl
        - apt-transport-https
    - name: create project path
      file: path={{ proj_path }} state=directory
      become: true
    - name: install and configure OpenLDAP
      include_role:
        name: ansible-ldap-role
~~~

Directory hierarchy

~~~
$ tree -L 3
.
├── ansible.cfg
├── hosts
└── playbooks
    ├── devel.yml
    └── roles
            └── ansible-ldap-role
~~~

**RUN**:
ansible-playbook playbooks/devel.yml
