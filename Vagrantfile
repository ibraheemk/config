# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
echo I am provisioning...
date > /etc/vagrant_provisioned_at
SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provision "shell", inline: $script
  config.vm.box = "hashicorp/precise64"
  config.vm.hostname = "vagrant.dev"
  config.vm.define "dev_vm" do |dev_vm|
  end
 
  # PostgreSQL Server port forwarding
  config.vm.network "forwarded_port", guest: 5432, host: 15432
  # Flask port forwarding
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"


  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./", "/srv/flask"
  config.vm.synced_folder "./script/", "/vagrant/script"

  config.vm.provider "virtualbox" do |vb|
  # Don't boot with headless mode
    vb.gui = false
    vb.name = "dev_vm"
 
    # Use VBoxManage to customize the VM. For example to change memory:
   #   vb.customize ["modifyvm", :id, "--memory", "1024"]
   end
  
  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file default.pp in the manifests_path directory.
  #
  config.vm.provision "puppet" do |puppet|
    puppet.manifests_path = "puppet/manifests"
    puppet.module_path  = "puppet/modules"
    puppet.manifest_file  = "site.pp"
  end

  config.vm.provision "shell", inline: "python /srv/flask/app.py &"
  config.vm.provision "shell", inline: "/vagrant/script/postgres.sh"
  
  
end
