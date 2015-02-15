# Installing Ansible

I am using Ansible installed from source on OSX.

Installation is performed as described here:
http://ansibleworks.com/docs/intro_installation.html#running-from-source

## Install summary

To install from source.

    $ git clone git://github.com/ansible/ansible.git
    $ cd ./ansible
    $ source ./hacking/env-setup

If you don’t have pip installed in your version of Python, install pip:

    $ sudo easy_install pip

Ansible also uses the the following Python modules that need to be installed:

    $ sudo pip install paramiko PyYAML jinja2
