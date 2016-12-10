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
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  #compute.vm.box_url = "./virtualbox.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Forward ODL's web GUI (DLUX) port so it's accessible on the host machine
  l2switch.vm.network "forwarded_port", guest: 8181, host: 8181

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  num_nodes = (ENV['NUM_NODES'] || 1).to_i

  # ip configuration
  ip_base = (ENV['SUBNET'] || "192.168.50.")
  ips = num_nodes.times.collect { |n| ip_base + "#{n+75}" }

  num_nodes.times do |n|
    config.vm.define "meetupvm#{n+1}", autostart: true do |compute|
      vm_ip = ips[n]
      vm_index = n+1
      compute.vm.hostname = "meetupvm#{vm_index}"
      compute.vm.network "private_network", ip: "#{vm_ip}"
      compute.vm.provider :virtualbox do |vb|
        vb.memory = 4096
        vb.customize ["modifyvm", :id, "--ioapic", "on"]      
        vb.cpus = 1
      end
    end
  end
  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
    apt-get install -y git-core git > /dev/null
    apt-get install -y vim > /dev/null
    apt-get install openvswitch-switch
    apt-get -y install mininet
    apt-get -y install default-jdk

    #
    # install OpenDaylight Boron SR1 release
    cp /vagrant/distribution-karaf-0.5.1-Boron-SR1.tar.gz ~
    unzip -xzvf ~/distribution-karaf-0.5.1-Boron-SR1.tar.gz

    # Install Eclipse IDE
    #cp /vagrant/eclipse-jee-mars-1-linux-gtk-x86_64.tar.gz ~
    #tar -xzvf ~/eclipse-jee-mars-1-linux-gtk-x86_64.tar.gz
    #sed -i 's/-XX:MaxPermSize=256m/-XX:MaxPermSize=512m/g' ~/eclipse/eclipse.ini
    #sed -i 's/-Xmx1024m/-Xmx2048m/g' ~/eclipse/eclipse.ini

    # Clone Layer 2 Switch repo:
    #git clone https://git.opendaylight.org/gerrit/p/l2switch.git -b stable/boron


  SHELL
end
