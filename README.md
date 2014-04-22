# Ansible Supervisor
A playbook role to install and configure `supervisord` and `supervisorctl` on Centos/Red Hat/Fedora with Ansible

## Installation
- Clone this repository inside your `roles` directory as `supervisor`
or add it as a submodule: `git submodule add git@github.com:Vinelab/ansible-supervisor roles/supervisor`

- In your playbook:

```yaml
roles:
  - supervisor
```

## Synopsis
Supervisor will be installed using python's `easy_install` that is included in the setup tools.

## Vars
The following configuration is required:

> programs values should not have spaces b/w `=`

```yaml
vars:
  supervisor:
      config:
          file: /etc/supervisord.conf
          dir: /etc/supervisord.d
      log:
          dir: /var/log/supervisord
      status:
        file: status.ini
        port: 127.0.0.1:9001
        username: user
        password: 123

      programs: # define your programs here, i.e:
        - file: web.ini
          name: nginx
          values:
            - command=/usr/sbin/nginx
        - file: web.ini
          name: php-fpm
          values:
            - command=/usr/bin/php-fpm
            - autostart=true
        - file: db.ini
          name: redis
          values:
            - command=/usr/init.d/redis start
```
