# -*- mode: ruby -*-
# vi: set ft=ruby :

NUM_NODE = 3
IP_NW = "192.168.5."
MASTER_IP_START = 10
NODE_IP_START = 20

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "generic/centos7"
  config.vm.box_check_update = false

  (1..3).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.hostname = "node-#{i}"
      config.vm.provider "virtualbox" do |vb|
        # vb.name = "node-#{i}"
        vb.memory = 1024
        vb.cpus = 1
      end
      
      ## IP will be 192.168.5.11
      ##            192.168.5.12
      ##            192.168.5.13
      node.vm.network :private_network, ip: IP_NW + "#{MASTER_IP_START + i}"
    end
  end
end
