ejabberd Development Environment
================================

This project will help you create a VM preconfigured with a
environment ready to jump start ejabberd development.

It will have configured all the database back-end supported by
ejabberd, so that you can run tests against all back-ends while
developing.

## Installation

* Install [VirtualBox](https://www.virtualbox.org/) (free) or VMWare
  Fusion (paid)
* Install [Vagrant](http://downloads.vagrantup.com/)
* For VMWare Fusion, Vagrant team is selling an extension to support
  [VMWare Vagrant add-on](http://www.vagrantup.com/vmware)
* Install Ansible (either
  [clone](http://www.ansibleworks.com/docs/intro_installation.html#running-from-source),
  or `brew install ansible` seems to work for me, but you may have to
  `brew doctor` first)

## Running the VM

Commands:

* Use default vagrant SSH key (added by Vagrant at install time)
This is quick and easy setup, but insecure. Use a secure key if you plan
to expose that VM to network (Outside your development machine)

        ssh-add ~/.vagrant.d/insecure_private_key

* Start Vagrant infrastructure:

        vagrant up --provider=vmware_fusion

or if you want VirtualBox:

	ulimit -n 4096 # see https://github.com/mitchellh/vagrant/issues/2435
	vagrant up --provider=virtualbox

Note: if you have only VirtualBox or VMWare, no need to pass the provider arguments, it will be detected automatically (I think).

* (Optional) Make sure SSH Agent is set to use your main key, with forward, to be able to clone git repository inside the VM:

        ssh-add
        export ANSIBLE_SSH_ARGS="-o ForwardAgent=yes"
        export ANSIBLE_TRANSPORT="ssh"

* Create ejabberd development environment:

You can simply ask vagrant to run Ansible to install and configure the needed software:

    vagrant provision

Alternatively, you can manually run Ansible play

    ansible-playbook --connection ssh -u vagrant -i ansible/ansible.vmhosts ansible/playbooks/ejabberd_dev/bootstrap.yml

Note: I force the use of SSH instead of paramiko, as SSH option seems more reliable.

# Building ejabberd to run ejabberd tests

Typically, you can check out the ejabberd code in the projects
directory. It will be accessible in the VM (at least for VMware) from
the shared folder /projects:

    git clone git@github.com:processone/ejabberd.git projects/ejabberd

From the VM, you can then build ejabberd, with all dependencies
enabled for development:
    
    ./autogen.sh && ./configure --enable-mysql --enable-pgsql --enable-riak --enable-sqlite --enable-elixir --enable-tools 
    make
    make test

Known issues
============

For VirtualBox, I struggled a bit with this one:

- [Too many open files](https://github.com/mitchellh/vagrant/issues/2435) (caused by synced folder, while using VirtualBox)

Useful learning resources
=========================

Ansible is the tool used to create the recipes for automatic
deployment. Here are useful resources:

- [20 minutes Ansible screencast](http://www.ansibleworks.com/quickstart-video/)

Clean-up of previous Vagrant version
====================================

Before having an executable installer, Vagrant was published as a ruby gem.

I had to do a bit of clean-up to make things working, here is what I had to do.

- verify where the vagrant binary sits (if `which vagrant` returns a gem path, you are on an old version)
- remove the gem `gem uninstall vagrant` if this is the case
- `rm -rf ~/.vagrant.d` (I had some weird error messages and no really useful box, so I imploded it)
- then install using the [installer](http://downloads.vagrantup.com/)

Also useful: http://docs.vagrantup.com/v2/installation/uninstallation.html

Notes on box downloads
======================

For me downloading a Vagrant box was a bit slow (40KBytes/sec).

To save time I tried using [Axel](http://axel.alioth.debian.org/) (`brew install axel`) and got a 5x+ improvement (270KBytes/sec).

I used the following steps for the VirtualBox box download:

- `axel -n 5 http://files.vagrantup.com/precise64.box`
- `vagrant box add precise64 --provider virtualbox precise64.box`

I verified the box url [here](http://www.vagrantbox.es/).
