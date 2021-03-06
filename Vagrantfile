# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below.
Vagrant.configure("2") do |config|

  # Require YAML module
  require 'yaml'

  # needed this for git clone
  config.ssh.forward_agent = true

  # Read config.yml
  configParams = YAML.load_file('config.yml')

  # Every Vagrant development environment requires a box. 
  config.vm.box = configParams["general"]["name"]

  # Check for box update
  config.vm.box_check_update = configParams["general"]["update"]

  # Create a private network, which allows host-only access to the machine using a specific IP.
  config.vm.network configParams["network"]["networkType"],bridge: configParams["network"]["bridgedAdapter"],ip: configParams["network"]["ip"]

  # Share any additional folder to the guest VM. 
  configParams["sharedFolders"].each do |sharedFolder|
    config.vm.synced_folder sharedFolder["source"], sharedFolder["dest"]
  end

  config.vm.provider configParams["provider"]["type"] do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = configParams["provider"]["gui"]
  
    # Customize the amount of memory on the VM:
    vb.memory = configParams["provider"]["ram"]
  end

  # Enable provisioning with a shell script.
  config.vm.provision "shell", inline: 
   #{}"yum update -y
    "yum install -y epel-release

    if [ ! -f /usr/bin/ansible ]
        then
        yum install ansible -y
    fi

    ansible-playbook --private-key=.vagrant/machines/default/virtualbox/private_key -u vagrant #{configParams['sharedFolders'][0]['dest']}/main.yml
    "



end
