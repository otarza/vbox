# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Box / OS
VAGRANT_BOX = 'bento/ubuntu-19.10'
# Memorable name for your
VM_NAME = 'vbox'

# VM User — 'vagrant' by default
VM_USER = 'vagrant'

# Username on your Mac
MAC_USER = 'otarzakalashvili'

# Host folder to sync
HOST_PATH = '/Users/otarzakalashvili/develop'

# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/opt/develop'

# # VM Port — uncomment this to use NAT instead of DHCP
# VM_PORT = 8080
Vagrant.configure(2) do |config|

  # Vagrant box from Hashicorp
  config.vm.box = VAGRANT_BOX

  # Actual machine name
  config.vm.hostname = VM_NAME

  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 3076
    v.cpus = 2
  end

  #DHCP — comment this out if planning on using NAT instead
  #config.vm.network "private_network", type: "dhcp"
  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.network :forwarded_port, guest: 8025, host: 8025
  config.vm.network :forwarded_port, guest: 3306, host: 3306

  config.ssh.forward_agent = true

  # # Port forwarding — uncomment this to use NAT instead of DHCP
  # config.vm.network "forwarded_port", guest: 80, host: VM_PORT
  # Sync folder
  config.vm.synced_folder HOST_PATH, GUEST_PATH

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder '.', '/home/'+VM_USER+'', disabled: true

  #Install package for your VM
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -y
    apt-get install -y git
    apt-get install -y apt-transport-https
    apt-get install -y build-essential
    apt-get install -y curl
    apt-get install -y gnupg-agent
    apt-get install -y software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    apt-key fingerprint 0EBFCD88
    add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"
    apt-get update -y
    apt-get install -y docker-ce docker-ce-cli containerd.io
    curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
  SHELL
end
