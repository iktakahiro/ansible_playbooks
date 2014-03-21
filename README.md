# Ansible Playbooks

## What is this

[Ansible](http://www.ansible.com/home)で各種ソフトウェアを導入するPlaybookです。

## Dependencies

- ansible 1.4.5 and higher

## Target package list

- Python (2 and 3)
- Java
- Tomcat
- MySQL
- PostgreSQL
- Nginx
- PHP

## How to use

When you want to install Python3.3 and PostgreSQL, take the following steps.

### Edit ./hosts

```
[test-servers]
192.168.10.7
```

### Edit ./test.yaml

```yaml
---
- hosts: test-servers
  sudo: yes
  user: ec2-user
  vars_files:
    - roles/common/vars/vars.json
    - roles/package/vars/vars.json
  roles:
    - test
```

### Edit ./roles/test/tasks/main.yml

```yaml
--
- include: ../../common/tasks/common-packages.yml
- include: ../../package/tasks/python3.yml
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

```
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
│   │   ├── epel.yml
│   │   ├── main.yml
│   │   └── user_add.yml
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
│   │   ├── java7.yml
│   │   ├── mysql.yml
│   │   ├── nginx.yml
│   │   ├── php.yml
│   │   ├── postgresql.yml
│   │   ├── python2.yml
│   │   ├── python3.yml
│   │   ├── tomcat6.yml
│   │   └── tomcat7.yml
│   ├── templates
│   │   ├── conf
│   │   │   ├── my.cnf.j2
│   │   │   ├── php-fpm.conf.j2
│   │   │   └── php.ini.j2
│   │   ├── init
│   │   │   ├── init_mysql.j2
│   │   │   ├── init_nginx.j2
│   │   │   ├── init_php-fpm.j2
│   │   │   ├── init_postgres.j2
│   │   │   └── init_tomcat.j2
│   │   ├── logrotate
│   │   │   └── logrotate_nginx.j2
│   │   └── profile
│   │       ├── profile_java7.j2
│   │       ├── profile_mysql.j2
│   │       ├── profile_php.j2
│   │       ├── profile_postgresql.j2
│   │       ├── profile_python3.j2
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
