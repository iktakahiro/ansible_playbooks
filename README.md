# Ansible Playbooks

version 1.1.0
Released: 08-Jun-2014

## What is this

Playbooks of [Ansible](http://www.ansible.com/home).

If you want to learn about the Playbook, Please see the details below.

- [Playbooks - Ansible ](http://docs.ansible.com/playbooks.html)

## Dependencies

- ansible 1.6.1 and higher

## Target package list

- Python (2 and 3)
- Java
- Tomcat
- MySQL
- PostgreSQL
- Nginx
- PHP
- Ruby
- fluentd (not td-agent)
- Redis and Redis sentinel
- Zabbix-agent
- OpenSSL
- OpenSSH

## Target Linux Distribution

- [Amazon Linux](http://aws.amazon.com/jp/amazon-linux-ami/) 2013.09 and higher
- [CentOS](http://www.centos.org) 6 (need EPEL)

## How to use

When you want to install Python3.3 and PostgreSQL, take the following steps.

### Edit files

./hosts

```ini:hosts
[test-servers]
192.168.10.7
```

ignition/test//test.yaml

```yaml:test.yaml
---
- hosts: test-servers
  sudo: yes
  user: ec2-user
  vars_files:
    - ../../roles/common/vars/vars.json
    - ../../roles/package/vars/vars.json
  roles:
    - test
```

./roles/test/tasks/main.yml

```yaml:main.yml
--
- include: ../../common/tasks/common-packages.yml
- include: ../../package/tasks/python34.yml
- include: ../../package/tasks/postgresql.yml
```

### Run playbook

```sh
ansible-playbook -i hosts test.yml
```

### Result (Excerpt)

- yum
    - gcc
    - make
    - tmux
    - ncurses-devel
- build
    - /usr/local/python3/bin/python
    - /usr/local/python3/bin/pip
    - /usr/local/python3/bin/awscli
    - /usr/local/pgsql/bin/psql
- profile
    - /etc/profile.d/python3.sh
    - /etc/profile.d/postgresql.sh
- init script
    - /etc/init.d/postgres

## Tree (Just for reference)

This directory structure is based on the [Official Best Practies](http://docs.ansible.com/playbooks_best_practices.html). (But, It is very difficult to be beautiful struncture.)

```
├── group_vars
├── host_vars
├── hosts
├── ignition
│   └── test
│       └── test.yml
└── roles
    ├── common
    │   ├── defaults
    │   ├── files
    │   │   └── pub_key
    │   │       ├── test1.pub
    │   │       └── test2.pub
    │   ├── handlers
    │   │   └── main.yml
    │   ├── meta
    │   ├── tasks
    │   │   ├── common-packages.yml
    │   │   ├── common-users.yml
    │   │   ├── epel.yml
    │   │   ├── limit.yml
    │   │   └── local.yml
    │   ├── templates
    │   └── vars
    │       └── vars.json
    ├── package
    │   ├── defaults
    │   ├── files
    │   │   └── pub_key
    │   ├── handlers
    │   ├── meta
    │   ├── tasks
    │   │   ├── fluentd.yml
    │   │   ├── java7.yml
    │   │   ├── java8.yml
    │   │   ├── memcached.yml
    │   │   ├── mysql.yml
    │   │   ├── nginx.yml
    │   │   ├── nginx_with_passenger.yml
    │   │   ├── openssh.yml
    │   │   ├── openssl.yml
    │   │   ├── php.yml
    │   │   ├── postgresql.yml
    │   │   ├── python27.yml
    │   │   ├── python33.yml
    │   │   ├── python34.yml
    │   │   ├── redis-sentinel.yml
    │   │   ├── redis.yml
    │   │   ├── ruby21.yml
    │   │   ├── tomcat6.yml
    │   │   ├── tomcat7.yml
    │   │   └── zabbix-agent.yml
    │   ├── templates
    │   │   ├── conf
    │   │   │   ├── fluent.conf.j2
    │   │   │   ├── my56.cnf.j2
    │   │   │   ├── nginx.conf.j2
    │   │   │   ├── nginx_custom_http2tomcat.conf.j2
    │   │   │   ├── nginx_custom_https2php.j2
    │   │   │   ├── php-fpm.conf.j2
    │   │   │   ├── php.ini.j2
    │   │   │   ├── redis-sentinel.conf.j2
    │   │   │   ├── redis.conf.j2
    │   │   │   └── zabbix_agentd.conf.j2
    │   │   ├── init
    │   │   │   ├── init_fluentd.j2
    │   │   │   ├── init_memcached.j2
    │   │   │   ├── init_mysql.j2
    │   │   │   ├── init_nginx.j2
    │   │   │   ├── init_php-fpm.j2
    │   │   │   ├── init_postgres.j2
    │   │   │   ├── init_redis-sentinel.j2
    │   │   │   ├── init_redis.j2
    │   │   │   ├── init_tomcat.j2
    │   │   │   └── init_zabbix_agentd.j2
    │   │   ├── logrotate
    │   │   │   ├── logrotate_nginx.j2
    │   │   │   └── logrotate_zabbix_agentd.j2
    │   │   └── profile
    │   │       ├── profile_fluentd.j2
    │   │       ├── profile_java7.j2
    │   │       ├── profile_java8.j2
    │   │       ├── profile_memcached.j2
    │   │       ├── profile_mysql.j2
    │   │       ├── profile_php.j2
    │   │       ├── profile_postgresql.j2
    │   │       ├── profile_python27.j2
    │   │       ├── profile_python3.j2
    │   │       ├── profile_python33.j2
    │   │       ├── profile_python34.j2
    │   │       ├── profile_ruby21.j2
    │   │       ├── profile_tomcat.j2
    │   │       ├── setenv_tomcat6.j2
    │   │       └── setenv_tomcat7.j2
    │   └── vars
    │       └── vars.json
    └── test
        ├── defaults
        ├── files
        │   └── pub_key
        ├── handlers
        ├── meta
        ├── tasks
        │   └── main.yml
        ├── templates
        └── vars
            └── vars.json
```
