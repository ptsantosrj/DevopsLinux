# -*- mode: ruby -*-
# vi: set ft=ruby :

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
  #config.vm.box = "generic/debian9"
  #config.vbguest.no_remote = false
  # Disable automatic box update checking. If you disable this, then
  # b"generic/debian9"oxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

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
  # config.vm.network "private_network", ip: "192.168.33.10"
  #config.vm.network "private_network", type: "dhcp" #ip:"192.168.33.10"
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  #config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  #**********************Máquina controle ************************
  config.vm.define "controle" do |controle|
    controle.vm.box = "generic/debian9"
    controle.vm.network  "private_network", ip: "172.17.177.100"
    controle.vm.hostname = "controle"
    controle.vm.provider "virtualbox" do |vb|
      vb.name = "controle"
      vb.memory = "2048"
      vb.cpus = 2
    end
    controle.vm.provision "shell", path: "update.sh"
    controle.vm.synced_folder ".", "/vagrant", disabled: false
	controle.vm.synced_folder "./configs", "/var/configs", owner: "root", group: "root"
  end
  #************************Máquina Web************************
  config.vm.define "webserver" do |webserver|
    webserver.vm.box = "generic/debian9"
    webserver.vm.network  "private_network", ip: "172.17.177.101"
    webserver.vm.hostname = "webserver"
    webserver.vm.provider "virtualbox" do |vb|
      vb.name = "webserver"
      vb.memory = "512"
      vb.cpus = 1
    end
    webserver.vm.provision "shell", path: "update.sh"
    webserver.vm.synced_folder "./configs", "/var/configs", owner: "root", group: "root"
  end
#************************Máquina Banco de Dados************************
  config.vm.define "dataserver" do |dataserver|
    dataserver.vm.box = "generic/debian9"
    dataserver.vm.network  "private_network", ip: "172.17.177.102"
    dataserver.vm.hostname = "dataserver"
    dataserver.vm.provider "virtualbox" do |vb|
      vb.name = "dataserver"
      vb.memory = "512"
      vb.cpus = 1
    end
    dataserver.vm.provision "shell", path: "update.sh"
    dataserver.vm.synced_folder "./configs", "/var/configs", owner: "root", group: "root"
  end
#************************Máquina Master************************

  config.vm.define "master" do |master|
    master.vm.box = "generic/centos7"
    master.vm.network  "private_network", ip: "172.17.177.110"
    master.vm.hostname = "master"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master"
      vb.memory = "2048"
      vb.cpus = 2
    end
    master.vm.synced_folder "./configs", "/var/configs", owner: "root", group: "root"
  end
  (1..2).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "generic/centos7"
      node.vm.network "private_network", ip: "172.17.177.11#{i}"
      node.vm.hostname = "node#{i}"
      node.vm.provider "virtualbox" do |vb|
      vb.name = "node#{i}"
      vb.memory = "512"
      vb.cpus = 1
    end
    end   
  end
#************************Máquina Puppet Master************************

 config.vm.define "pupmaster" do |pupmaster|
    pupmaster.vm.box = "generic/centos7"
    pupmaster.vm.network  "private_network", ip: "172.17.177.105"
    pupmaster.vm.hostname = "pupmaster"
    pupmaster.vm.provider "virtualbox" do |vb|
      vb.name = "pupmaster"
      vb.memory = "2048"
      vb.cpus = 2
    end
  end
#************************Máquinas Puppet Agente************************

  config.vm.define "pupagent" do |pupagent|
    pupagent.vm.box = "geerlingguy/debian9"
    pupagent.vm.network  "private_network", ip: "172.17.177.106"
    pupagent.vm.hostname = "pupagent"
    #pupagent.vbguest.auto_update = false
    #pupagent.vbguest.no_remote = true
    pupagent.vm.provider "virtualbox" do |vb|
      vb.name = "pupagent"
      vb.memory = "512"
      vb.cpus = 1
    end
    pupagent.vm.provision "shell", inline: "apt update && apt install puppet -y"
    pupagent.vm.provision "puppet" do |puppet|
      puppet.manifests_path = "manifests"
      puppet.manifest_file = "default.pp"
    end
  end
#************************Máquina treinamento linux Geral************************

config.vm.define "docker" do |docker|
    docker.vm.box = "ubuntu/xenial64"
    docker.vm.network  "private_network", ip: "172.17.177.120"
    docker.vm.hostname = "docker"
    docker.vm.provider "virtualbox" do |vb|
      vb.name = "docker"
      vb.memory = "2048"
      vb.cpus = 2
    end
  end

#**********************************************************************************
# Instalar plugin group vagrant plugin install vagrant-group
#vagrant group up <group-name>
#vagrant group up <group-name> --provision
#vagrant group halt <group-name> --force
#vagrant group destroy <group-name>
#  config.group.groups = {
#    "controle" => [
#      "controle",
#      ],
#    "webserver" => [
#      "webserver",
#    ],
#    "dataserver" => [
#      "dataserver",
#    ],
#    "master" => [
#      "master",
#    ],
#     "nodes" => [
#      "node1",
#      "node2",
#      "node3",
#    ],
#  }
end
