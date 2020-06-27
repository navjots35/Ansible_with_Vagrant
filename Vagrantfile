# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.define "MainNode" do |mn|
    mn.vm.box = "centos/7"
    mn.vm.hostname = "server.MainNode.com"
    mn.vm.network "private_network", ip: "192.168.33.10"
  #  mn.vm.synced_folder "./ansible_with_vagrant", "/home/vagrant/share"
  end

  config.vm.define "WorkerNode" do |wn|
    wn.vm.box = "centos/7"
    wn.vm.hostname = "client.WorkerNode.com"
    wn.vm.network "private_network", ip: "192.168.33.11"
  #  wn.vm.synced_folder "./ansible_with_vagrant/workerNode", "/home/vagrant/share"
  end
end
