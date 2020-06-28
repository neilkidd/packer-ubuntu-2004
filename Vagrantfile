# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "virtualbox" do |virtualbox|

    # Change the box location to your local box
    virtualbox.vm.box = "file://builds/virtualbox-ubuntu2004.2004.20200628.082705.box"

    # Point to the cloud release
    # virtualbox.vm.box = "neil_kidd/ubuntu2004"

    config.vm.provider :virtualbox do |v|
      v.gui = true
      v.memory = 1024
      v.cpus = 2
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--ioapic", "on"]
      v.customize ["modifyvm", :id, "--rtcuseutc", "on"]
    end

    config.vm.provision "shell", inline: "echo Hello, Ubuntu on Vagrant "
  end

end
