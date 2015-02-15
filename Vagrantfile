# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.host_name = "vagrant.vm"
  config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "projects", "/projects"

  # MR: I will test / debug provisioning directly with Ansible commands
  config.vm.provision :ansible do |ansible|
    ansible.playbook = "ansible/playbooks/ejabberd_dev/bootstrap.yml"
    #ansible.inventory_path = "ansible/ansible.vmhosts"
    ansible.verbose = true
    ansible.host_key_checking = false
#    ansible.groups = {
#      "all" => ["vagrant.vm"]
#    }
  end

  config.vm.provider "vmware_fusion" do |v|
    # v.gui = true # Tmp to see error in vmware
    v.vmx['ethernet0.virtualDev'] = 'vmxnet3'
    v.vmx['ethernet1.virtualDev'] = 'vmxnet3'
    v.vmx["memsize"] = "2048"
    v.vmx["numvcpus"] = "2"
    config.vm.box     = "precise64_vmware"
    config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"
  end
  
  config.vm.provider "virtualbox" do |v|
    #   # Don't boot with headless mode
    #   vb.gui = true
    #
    #   # Use VBoxManage to customize the VM. For example to change memory:
    #   vb.customize ["modifyvm", :id, "--memory", "1024"]
    config.vm.box     = "precise64"
    config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  end

end
