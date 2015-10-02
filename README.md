# Ansible Jenkins plugin for Fedora

## What is it for ?

The aim of this role is to provide a complete possibility to provision a jenkins on a Fedora-like system.
With this role you'lle be able to :
* Install jenkins rpm and configure it (runas, port, prefix...)
* Download and install plugins
* Create jobs with providing their XML configuration
* Add nodes
* Add credentials

## Usage

First of all, install files into `/etc/ansible/roles/ansible-jenkins`, your directory tree should look like this :

    /etc/ansible/roles/ansible-jenkins
    |
    |_ handlers
    |  |_ main.yml
    |
    |_ tasks
    |  |_ credentials.yml
    |  |_ jenkins.yml
    |  |_ jobs.yml
    |  |_ main.yml
    |  |_ nodes.yml
    |  |_ plugins.yml
    |
    |_ templates
    |  |_ jenkins.j2
    |
    |_ vars
       |_ main.yml

You also have to install a little custom module called _yum_version_ inside the _library_ folder you defined in `/etc/ansible/ansible.cfg` : 
```properties
library        = /usr/share/ansible/
```

Next, let assume you have jenkins group section configured in `/etc/ansible/hosts`, you can now configure groups variables in `/etc/ansible/group_vars/jenkins` this way : 
``` yml
---
jenkins_port: 9090
jenkins_version: 1.620-1.1
plugins:
  git:
    name: git
    version: 2.4.0
  ansicolor:
    name: ansicolor
    version: 0.4.0
credentials: /vagrant/credentials.xml
nodes:
  slave1:
    name: slave1
    config_path: /vagrant/slave1.xml
jobs:
  job1:
    name: job1
    config_path: /vagrant/job1.xml
  job2:
    name: job2
    config_path: /vagrant/job2.xml
```

In order to run it, you just have to include this role in your playbook, for instance : 
``` yml
---
- hosts: jenkins
  sudo: yes

  pre_tasks:
  - name: ensure java8 installed
    yum: name=java-1.8.0-openjdk-devel state=present
  - name: ensure firewall is off
    service: name=firewalld state=stopped enabled=false

  roles:
    - ansible-jenkins
```
