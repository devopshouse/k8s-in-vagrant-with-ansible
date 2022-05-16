# -*- mode: ruby -*-
# vi: set ft=ruby :

JOIN_COMMAND_FILE = "kubernetes-setup/join-command"
IMAGE_NAME = "generic/ubuntu2004"
NETWORK_PREFIX="10.69.40"
NODES = 2

Vagrant.configure("2") do |config|
  # disabling nfs-export
  config.nfs.functional = false

  # disable vbguest update
    if Vagrant.has_plugin?("vagrant-vbguest")
      config.vbguest.auto_update = false
    end

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 2
    end

    # set network tp dhcp
    config.vm.network "private_network", type: "dhcp"

    config.vm.define "master" do |master|
        master.vm.network "public_network", ip: "#{NETWORK_PREFIX}.10", netmask:"255.255.0.0", bridge: "en0: Ethernet"
        master.vm.box = IMAGE_NAME
        master.vm.hostname = "master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "#{NETWORK_PREFIX}.10",
                lb_address_pool: "#{NETWORK_PREFIX}.150-#{NETWORK_PREFIX}.250",
            }
        end
    end

    (1..NODES).each do |i|
        config.vm.define "node#{i}" do |node|
            node.vm.network "public_network", ip: "#{NETWORK_PREFIX}.#{i + 10}", netmask:"255.255.0.0", bridge: "en0: Ethernet"
            node.vm.box = IMAGE_NAME
            node.vm.hostname = "node#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "#{NETWORK_PREFIX}.#{i + 10}",
                }
            end
        end
    end

    # delete local join_command file
    File.delete(JOIN_COMMAND_FILE) if File.exist?(JOIN_COMMAND_FILE)
end
