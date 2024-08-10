VSFTPD
=========

Installs VSFTPD on Debian.

Based on:  
https://www.alibabacloud.com/help/en/ecs/how-to-construct-vsftp-and-configure-virtual-users  
https://sources.debian.org/src/vsftpd/3.0.3-8/EXAMPLE/VIRTUAL_USERS/README/  
https://infoloup.no-ip.org/ftps-vsftpd-debian12/  


Requirements
------------

Debian OS.

Role Variables
--------------

Use

Define users.

`name` and `password` are required.

`config` is optional.  If not defined, the `vsftpd_default_user_config` will be used.

You can use all variables from the specification:  
https://linux.die.net/man/5/vsftpd.conf

```
vsftpd_virtual_users:
- name: ftpuser1
  password:
  config: |
    local_root={{ vsftpd_user_home }}/{{ item.name }}
```


Those users may see the whole file system.

```
vsftpd_users_excluded_from_chroot_jail:
- ftpuser1
```

Users can be temporarily disabled.

```
vsftpd_users_temporary_disabled:
- ftpuser1
```


Dependencies
------------

No dependencies.

Example Playbook
----------------

The following task order should be followed.

- install.yml
- user.yml
- configure.yml

Install vsftpd

```
- name: Install vsftpd
  hosts: all
  gather_facts: false
  roles: 
  - ansible-role-vsftpd
  vars:
    vsftpd_rsa_cert_file: /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/example.com/example.com.crt
    vsftpd_rsa_private_key_file: /var/lib/caddy/.local/share/caddy/certificates/acme-v02.api.letsencrypt.org-directory/example.com/example.com.key
    vsftpd_virtual_users:
    - name: ftpuser1
      password: cleartext1
    - name: ftpuser2
      password: cleartext2
```

Invoke particular tasks using `tags`, or by invoking them in a playbook, as below.

```
- name: Install vsftpd
  hosts: all
  gather_facts: false
  tasks:
  - name: Install vsftpd
    import_role:
      name: ansible-role-vsftpd
      tasks_from: install.yml
```

License
-------

MIT

Author Information
------------------

(c) 2024 Pawel Idzikowski  
Wincognito
