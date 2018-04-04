# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end
#development environment
  config.vm.define "gDev" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "gDev"
    d.vm.network "private_network", ip: "10.100.198.200"
    #install ansible
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    #set up environent
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/gDev.yml -c local"
    #check out calcApp code
    d.vm.provision :shell, path: "scripts/importCalc.sh"
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end
#production
  config.vm.define "gProd" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "gProd"
    d.vm.network "private_network", ip: "10.100.198.201"
    d.vm.provider "virtualbox" do |v|
      v.memory = 1024
    end
  end
#service discovery
  (1..3).each do |i|
    config.vm.define "gSvDisc0#{i}" do |d|
      d.vm.box = "ubuntu/trusty64"
      d.vm.hostname = "gSvDisc0#{i}"
      d.vm.network "private_network", ip: "10.100.194.20#{i}"
      d.vm.provider "virtualbox" do |v|
        v.memory = 1024
      end
    end
  end
#reverse proxy service  
  config.vm.define "gProxy" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "gProxy"
    d.vm.network "private_network", ip: "10.100.193.200"
    d.vm.provider "virtualbox" do |v|
      v.memory = 1024
    end
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = true
    config.vbguest.no_remote = true
  end
  ###############################################################################
  ###############################################################################
  #tf development environment
  config.vm.define "tfDev" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "tfDev"
    d.vm.network "private_network", ip: "10.100.198.202"
    #install ansible
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    #set up environent
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/tfDev.yml -c local"
    #check out tf code
    d.vm.provision :shell, path: "scripts/importTF.sh"
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end
  
end
