yum-security
=========

- yum-security for installing yum security patches only

Requirements
------------

Needs SSH keys on the destination server plus sudo access.

Role Variables
--------------

Currently none

Dependencies
------------

Currently none

Example Playbook
----------------

```YAML
---
- hosts: all
#serial: 1 # only do a single host at a time
  become: yes
  pre_tasks:
    - name: make sure ansible_lsb is there
      yum: name=redhat-lsb-core state=present
      when: ansible_os_family == "RedHat"
        #and ansible_lsb.major_release | int >=6

    - name: Reread ansible_lsb facts
      setup: filter=ansible_lsb*
      when: ansible_os_family == "RedHat"
        #and ansible_lsb.major_release | int >=6

  roles:
    - yum-security
  vars:
    - doupdate: "no"
```
An example run to ''show'' only outstanding security patches could be;

```YAML
ansible-playbook securityupdate.yml --limit somehostgroup
```

and to apply updates;
```YAML
ansible-playbook securityupdate.yml --limit somehostgroup --extra-vars "dorestart='yes'"
```

License
-------

BSD


