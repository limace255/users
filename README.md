Role Name
=========

Managing users on UNIX Operating Systems.
Inspired by franklinkim's role + geerlinguy

Requirements
------------

Role Variables
--------------

No variables in vars/main.yml file. Everything is in default.yml file.
Most of the parameters are from "native" Ansible user module.

#---
## defaults file for users
## Create a group for each user and make that their primary group
#users_create_per_user_group: true
## Default shell 
#users_default_shell: /bin/bash
## Default home dir creation (false in case of mounted FS like Solaris)
#users_create_homedirs: true
## Users homedir
#users_homedir: /home
#users_ssh_key_type: rsa
#users_ssh_key_bits: 4096
#users_authorized_key_exclusive: no
#
## Lists of users to create and delete
#users: []
#users_group: []
#users_groups: []
#users_to_remove: []
#groups_to_remove: []
#
## List of groups to create
## Example:
#groups_to_create:
#  - name: teamrocket
#    gid: 255

Dependencies
------------

Example Playbook
----------------

- hosts: all
  become: yes
  roles:
    - limace255.users
  vars:
    # Main Group
    groups_to_create:
      - name: admin-group
        gid: 255
      - name: users-group
        gid: 1256
      - name: web-admin
        gid: 2000
      - name: www-data
        gid: 33
      - name: backup
        gid: 501
    # Main users
    users:
      - name: "Super Admin"
        username: admin
        uid: 255
        password: "super_hashed_password"
        group: admin-group
        groups: ["users-group"]
        shell: /bin/zsh
      - name: "Global User"
        username: global-user
        uid: 1256
        password: "super_hashed_password_but_different_from_the_first_one"
        group: "users-group"
        groups: ["web-admin", "games"]
      - name: "www-data"
        username: "www-data"
        uid: 33
        group: "www-data"
        home: "/var/www" 
      - name: "Backup User"
        username: infra-backup
        uid: 501
        group: "backup"
        groups: ["web-admin", "www-data"]
        ssh_privkey: "{{ lookup('file', 'ssh_files/backup_rsa') }}"
        ssh_key:
          - "ssh-rsa pub key"

    users_to_remove:
      - username: dtrump
      - username: nsa-master

License
-------

BSD

Author Information
------------------

Created by limace255 

