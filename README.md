[![Build Status](https://travis-ci.org/singleplatform-eng/ansible-role-celery.svg?branch=master)](https://travis-ci.org/singleplatform-eng/ansible-role-celery)
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
celery_bin: "/usr/local/bin/celery"
```
Required Vars
```
celery_working_dir
celery_app
celery_user
celery_services
```
celery_services should be a dict that contains the name of the service and any environment variables necessary in addition to {{celeryd_default_env}}. The template merges the two dictionaries if additional environment variables are defined. Note that quotes are not explicitly included in the EnvironmentFile template, so if quotes are needed they should be explicitly defined.

Dependencies
------------
Python2.7+

Message Broker - the tests in this role use a local instance of redis-server

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
	    state: started
	    enabled: yes
          - name: celery_beat
	    state: stopped
	    enabled: no
            environment:
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
