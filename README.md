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
Supervisor will be installed using python's `easy_install`
that is included in the setup tools.

## Usage

> programs values should not have spaces b/w `=`

### Programs

```yaml
vars:

  supervisor:
    programs:
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

### Optional Configuration
You may override any of the default `supervisord` configuration set by adding
any of the following variables.

> Note: any overridden variable must introduce all of its attributes.

```yaml
supervisord:
  runtime:
    dir: /var/run/supervisor
    nodaemon: "false"
    socket: supervisord.sock
    pidfile: supervisord.pid

  config:
    dir: /etc/supervisord.d
    file: /etc/supervisord.conf
    default: /etc/default.supervisord.conf

  log:
    dir: /var/log/supervisord
    file: supervisord.log
    level: info

  http:
    file: status.ini
    port: 127.0.0.1:9002
    username: user
    password: 123
```
