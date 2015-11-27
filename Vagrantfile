# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # defaults for all nodes
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider :virtualbox do |vb|
    vb.gui = false
    vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", "2"]
  end

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  config.ssh.forward_agent = true

  node1_name = "lfc01" 
  config.vm.define node1_name do |node|
    node.vm.hostname = node1_name
    node.vm.network "private_network", ip: "10.10.1.10"
    node.vm.provider :virtualbox do |vb|
      vb.name = node1_name
      vb.customize ["createhd",  "--filename", "lfc01_disk0", "--size", "2048"]
      vb.customize ["createhd",  "--filename", "lfc01_disk1", "--size", "2048"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "1", "--type", "hdd", "--medium", "lfc01_disk0.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "2", "--type", "hdd", "--medium", "lfc01_disk1.vdi"]
    end
  end

  node2_name = "lfc02"
  config.vm.define node2_name do |node|
    node.vm.hostname = node2_name
    node.vm.network "private_network", ip: "10.10.1.20"
    node.vm.provider :virtualbox do |vb|
      vb.name = node2_name
      vb.customize ["createhd",  "--filename", "lfc02_disk0", "--size", "1024"]
      vb.customize ["createhd",  "--filename", "lfc02_disk1", "--size", "1024"]
      vb.customize ["createhd",  "--filename", "lfc02_disk2", "--size", "1024"]
      vb.customize ["createhd",  "--filename", "lfc02_disk3", "--size", "1024"]
      vb.customize ["createhd",  "--filename", "lfc02_disk4", "--size", "1024"]
      vb.customize ["createhd",  "--filename", "lfc02_disk5", "--size", "1024"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "1", "--type", "hdd", "--medium", "lfc02_disk0.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "2", "--type", "hdd", "--medium", "lfc02_disk1.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "3", "--type", "hdd", "--medium", "lfc02_disk2.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "4", "--type", "hdd", "--medium", "lfc02_disk3.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "5", "--type", "hdd", "--medium", "lfc02_disk4.vdi"]
      vb.customize ["storageattach", :id, "--storagectl", "SATAController", "--port", "6", "--type", "hdd", "--medium", "lfc02_disk5.vdi"]
    end
  end
end
