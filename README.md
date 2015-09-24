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

First of all, install files into `/etc/ansible/roles/ansible-jenkins`, your arborescenc should look like this : 
'''
ansible-jenkins
|
|_ handlers
|   |_ main.yml
|
|_ tasks
|  |_ credentials.yml

'''

Let assume you have jenkins group section configured in `/etc/ansible/hosts`, you can now configure
