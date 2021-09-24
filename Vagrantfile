# -*- mode: ruby -*-
# vi: set ft=ruby :

IPADDR = "192.168.128.3"
HOSTNAME = "develop.localdomain"
CPU = "2"
MOMERY = "2048"

COMPOSE_VERSION = "1.25.3"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false
  if Vagrant.has_plugin?("vagrant-vbguest")
    #config.vbguest.auto_update = false
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: IPADDR
  config.vm.hostname = HOSTNAME

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../php7", "/php7", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../frontendjs", "/frontendjs", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../nodejs12", "/nodejs12", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../serverlessjs10", "/serverlessjs10", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../terraform12", "/terraform12", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../docker-awscli", "/docker-awscli", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../cakephp-aws-s3bucket", "/cakephp-aws-s3bucket", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../corporate", "/corporate", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../webuke-jp", "/webuke-jp", type: "virtualbox", mount_options: ["ttl=0"]
  config.vm.synced_folder "../hyakuka-jp", "/hyakuka-jp", type: "virtualbox", mount_options: ["ttl=0"]

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.customize ["modifyvm", :id, "--cpus", CPU, "--ioapic", "on"]
    vb.customize ["modifyvm", :id, "--memory", MOMERY]
    vb.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]

    # Customize for workarounds
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["setextradata", :id, "VBoxInternal/Devices/VMMDev/0/Config/GetHostTimeDisabled", "0"]
    vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate", "1"]
  end

  # View the documentation for the provider you are using for more
  # information on available options.
  # config.ssh.insert_key = false
  # config.ssh.forward_agent = true

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "docker" do |docker|
    docker.pull_images "busybox:latest"
  end

  config.vm.provision "install", type: "shell", inline: <<-SHELL
    curl -L "https://github.com/docker/compose/releases/download/#{COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod a+x /usr/local/bin/docker-compose
    echo 'if [ -f "$HOME/.dockerrc" ]; then source ${HOME}/.dockerrc; fi' > /etc/profile.d/dockerrc.sh
    echo 'export PATH=/vagrant/bin:$PATH' > /etc/profile.d/vagrant_bin.sh
  SHELL

  config.vm.provision "startup", type: "shell", run: "always", inline: <<-SHELL
    cp /vagrant/.dockerrc /home/vagrant/.dockerrc
    source /home/vagrant/.dockerrc && /usr/local/bin/docker-compose up -d --force-recreate
  SHELL

  config.vm.define :default, primary: true, autostart: true do |ubuntu|
    ubuntu.vm.box = "ubuntu/focal64"
  end
end
