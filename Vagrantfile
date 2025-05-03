# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "centos/stream9",
        :box_version => "20250331.0",
        :vm_name => "inetRouter",
        :net => [
                    #ip, adpter, netmask, virtualbox__intnet
                    #["192.168.255.1", 2, "255.255.255.252",  "router-net"], 
                  ["192.168.255.1", 2, "255.255.255.252" ,"router-net"],
                  ["0.0.0.0", 3, "255.255.255.255" ,"router-net"],
                  ["192.168.56.10", 8, "255.255.255.0"],
                ]
  },
  :centralRouter => {
        :box_name => "centos/stream9",
        :box_version => "20250331.0",
        :vm_name => "centralRouter",
        :net => [
                  ["192.168.255.2", 2, "255.255.255.252" ,"router-net"],
                  ["0.0.0.0", 3, "255.255.255.255" ,"router-net"],
                  ["192.168.255.9", 6, "255.255.255.252" ,"office1-central"],
                  ["192.168.56.11", 8, "255.255.255.0"],
                ]
  },

  :office1Router => {
        :box_name => "centos/stream9",
        :box_version => "20250331.0",
        :vm_name => "office1Router",
        :net => [
                  ["192.168.255.10", 2, "255.255.255.252" ,"office1-central"],
                  ["0.0.0.0", 3, "255.255.255.255" ,"vlan1"],
                  ["0.0.0.0", 4, "255.255.255.255" ,"vlan1"],
                  ["0.0.0.0", 5, "255.255.255.255" ,"vlan2"],
                  ["0.0.0.0", 6, "255.255.255.255" ,"vlan2"],
                  ["192.168.56.20", 8, "255.255.255.0"],
                ]
  },

  :testClient1 => {
        :box_name => "centos/stream9",
        :box_version => "20250331.0",
        :vm_name => "testClient1",
        :net => [
                  ["0.0.0.0", 2, "255.255.255.255" ,"testLAN"],
                  ["192.168.56.21", 8, "255.255.255.0"],
                ]
  },

  :testServer1 => {
        :box_name => "centos/stream9",
        :box_version => "20250331.0",
        :vm_name => "testServer1",
        :net => [
                  ["0.0.0.0", 2, "255.255.255.255" ,"testLAN"],
                  ["192.168.56.22", 8, "255.255.255.0"],
            ]
  },

  :testClient2 => {
        :box_name => "ubuntu/jammy64",
        :box_version => "20241002.0.0",
        :vm_name => "testClient2",
        :net => [
                  ["0.0.0.0", 2, "255.255.255.255" ,"testLAN"],
                  ["192.168.56.31", 8, "255.255.255.0"],
                ]
  },

  :testServer2 => {
        :box_name => "ubuntu/jammy64",
        :box_version => "20241002.0.0",
        :vm_name => "testServer2",
        :net => [
                   ["0.0.0.0", 2, "255.255.255.255" ,"testLAN"],
                   ["192.168.56.32", 8, "255.255.255.0"],
                ]
  },

}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    
    config.vm.define boxname do |box|
   
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      box.vm.box_version = boxconfig[:box_version]
      box.vm.synced_folder ".", "/vagrant", disabled: true


      config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
       end

       if boxconfig[:vm_name] == "testServer2"
        box.vm.provision "ansible" do |ansible|
         ansible.playbook = "provision.yml"
         ansible.inventory_path = "staging/hosts"
         ansible.host_key_checking = "false"
         ansible.become = "true"
         ansible.limit = "all"
        end
       end
 
       boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
       end
 
       box.vm.provision "shell", inline: <<-SHELL
         mkdir -p ~root/.ssh
         cp ~vagrant/.ssh/auth* ~root/.ssh
       SHELL
     end
   end
 end
