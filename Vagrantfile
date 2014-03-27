# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

BOX_DIR = File.expand_path("~/.box")
BOX_NAME = "CentOS-6.5-x86_64-mininal"
BOX_PATH = File.expand_path("~/.box/#{BOX_NAME}.box")

if !FileTest.exists?("packer/builds")
  Dir.mkdir("packer/builds")
end

if !FileTest.exists?(BOX_PATH)
  system(%W(
    cd packer && 
    packer build template.json && 
    mkdir -p #{BOX_DIR} && 
    mv builds/#{BOX_NAME}.box #{BOX_PATH} 
  ).join(' '))
end

$stop_iptables = <<EOF
  service iptables stop
EOF

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  config.vm.box = BOX_NAME
  config.vm.box_url = BOX_PATH

  config.vm.define :client do |client|

    client.vm.network "private_network", ip: "192.168.33.150"
    client.ssh.forward_agent = true

    client.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--memory", "384"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    client.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.limit = "all"
      ansible.inventory_path = "vagrant.box.client"
      ansible.verbose = "vvvv"
    end

    client.vm.provision :shell, inline: $stop_iptables

  end

  config.vm.define :server do |server|

    server.vm.network "private_network", ip: "192.168.33.155"
    server.ssh.forward_agent = true

    server.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.customize ["modifyvm", :id, "--memory", "384"]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    server.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.limit = "all"
      ansible.inventory_path = "vagrant.box.server"
      ansible.verbose = "vvvv"
    end

    server.vm.provision :shell, inline: $stop_iptables

  end

end
