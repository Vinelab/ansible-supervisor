# Ansible Supervisor
A playbook role to install and configure `supervisord` and `supervisorctl` on Centos/Red Hat/Fedora with Ansible

## Installation
- Clone this repository inside your `roles` directory as `supervisor`
or add it as a submodule: `git submodule add git@github.com:Vinelab/ansible-supervisor roles/supervisor`

- In `ansible.cfg` make sure that `hash_behavior = merge` under `[defaults]`

- In your playbook:

```yaml
roles:
  - supervisor
```

## Synopsis
Supervisor will be installed using python's `easy_install`
that is included in the setup tools.

## Usage

> `hash_behavior` must be set to `merge` in your `ansible.cfg` under `[defaults]`

### Programs

```yaml
vars:

  supervisor:
    programs:
      - file: web.ini
        name: nginx
        values:
          - command: /usr/sbin/nginx
      - file: web.ini
        name: php-fpm
        values:
          - command: /usr/bin/php-fpm
          - autostart: "true"
      - file: db.ini
        name: redis
        values:
          - command: /usr/init.d/redis start
      - files: queue.ini
        name: queue-sync
        values:
          - directory: /var/www/app
          - command: /usr/bin/php artisan queue:listen --queue=sync --sleep=20
```

you can have anything in the `values` section since the dictionary `key` will
represent the `option` and the `value` will be the option's `value`.

### Http Server
Configuring the http server credentials

> `hash_behavior` must be set to `merge` in your `ansible.cfg` under `[defaults]`

```yaml
supervisor:
  http:
    username: custom_username
    password: 123passwd
```

### Optional Configuration
You may override any of the default `supervisord` configuration set by adding
any of the following variables.

> `hash_behavior` must be set to `merge` in your `ansible.cfg` under `[defaults]`

```yaml
supervisor:
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
