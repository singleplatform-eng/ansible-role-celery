Ansible-Role-Celery
=========

This role is designed to provide necessary Environment Files, and Service Units for managing Celery Services on Ubuntu running systemd. It does not manage the installation for celery, or manage celery users/groups.

Requirements
=========

Ubuntu 18.04+

Role Variables
--------------
Defaults
```
celery_env_dir: /etc/celery.d
celery_log_dir: /var/log/celery
celery_run_dir: /var/run/celery
celery_bin: "{{python_virtualenv}}/bin/celery"
```
Required Vars
```
celery_working_dir
celery_app
celery_user
celery_services
```
Dependencies
------------
Python
Python VirtualEnv

This role assumes that the celery binary is installed into the local python virtualenv's bin directory. The default value of celery_bin requires that the ansible variable "{{python_virtualenv}}" be defined.

Example Playbook
----------------

An example of how to use the role

    - hosts: localhost
      vars:
        python_virtualenv: /srv/python2.7
        celery_working_dir: /srv/django
        celery_app: django
        celery_user: celery
        celery_services:
          - name: celeryd
            environment:
              CELERYD_LOG_FILE: "/var/log/celery/%n%I.log"
              CELERYD_PID_FILE: "/var/run/celery/%n.pid"
              CELERY_APP: "{{celery_app}}"
              CELERY_BIN: "{{celery_bin}}"
          - name: celery_beat
            environment:
              CELERYD_LOG_FILE: "/var/log/celery/%n%I.log"
              CELERYD_PID_FILE: "/var/run/celery/%n.pid"
              CELERY_APP: "{{celery_app}}"
              CELERY_BIN: "{{celery_bin}}"
              SINGLE_BEAT_IDENTIFIER: celery-beat
              SINGLE_BEAT_LOCK_TIME: 300
    role:
      - { role: ansible-role-celery }

Author Information
------------------

[SinglePlatform Engineering](http://engineering.singleplatform.com/)

License
-------

BSD 3-Clause
