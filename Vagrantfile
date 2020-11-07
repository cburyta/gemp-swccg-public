# Vagrantfile for GEMP SWCCG development box
# -*- mode: ruby -*-
# vi: set ft=ruby :

# enable overrides with env vars
private_ip = ENV['GEMP_PRIVATE_IP'] || "192.168.50.94"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Name box "gemp"
  config.vm.define "gemp" do |gemp|
    gemp.vm.box = "bento/centos-7"

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    gemp.vm.network "private_network", ip: private_ip
    # gemp.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true

    gemp.vm.post_up_message = <<-HEREDOC
    Startup:
        vagrant ssh
        mvn clean install
        ./run-gemp.sh
    Web Address:
        http://#{private_ip}:8080/gemp-swccg
    HEREDOC

    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    config.vm.provider "virtualbox" do |vb|
      vb.name = 'gemp'

      # Customize the amount of memory on the VM:
      vb.memory = "2048"
    end

    # Enable provisioning with a shell script if not set up.
    box_location = "#{File.dirname(__FILE__)}/.vagrant/machines/gemp-swccg/virtualbox"
    if !Dir.exist?(box_location) or Dir.entries(box_location).length < 3 or ARGV[1] == '--provision'

      config.vm.provision :shell, :path => "vagrant-build/bootstrap.sh", :privileged => false
    end

  end
end
