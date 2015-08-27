# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure(2) do |config|
 
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |vb|
  
	vb.memory = "256"
  end

  config.vm.define :happyserver do |srv|
	srv.vm.hostname = "happyserver"
	srv.vm.network "private_network", ip: "10.11.12.13"

  end

  config.vm.define :happyclient do |client|
	client.vm.hostname = "happyclient"
	client.vm.network "private_network", ip: "10.11.12.13", auto_config: false

  end

end
