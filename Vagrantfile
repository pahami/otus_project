
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :Otussrv0 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "Otussrv0",
        #:public => {:ip => "10.10.10.1", :adapter => 1},
        :net => [   
                    #ip, adpter, netmask, virtualbox__intnet
                    #["192.168.255.1", 2, "255.255.255.252",  "router-net"], 
                    ["192.168.50.10", 8, "255.255.255.0"],
                ]
  },

  :Otussrv1 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "Otussrv1",
        :net => [
#                   ["192.168.255.2",  2, "255.255.255.252",  "router-net"],
#                   ["192.168.0.1",    3, "255.255.255.240",  "dir-net"],
#                   ["192.168.0.33",   4, "255.255.255.240",  "hw-net"],
#                   ["192.168.0.65",   5, "255.255.255.192",  "mgt-net"],
#                   ["192.168.255.9",  6, "255.255.255.252",  "office1-central"],
#                   ["192.168.255.5",  7, "255.255.255.252",  "office2-central"],
                   ["192.168.50.11",  8, "255.255.255.0"],
                ]
  },

  :Otussrv2 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "Otussrv2",
        :net => [
#                   ["192.168.0.2",    2, "255.255.255.240",  "dir-net"],
                   ["192.168.50.12",  8, "255.255.255.0"],
                ]
  },

  :Otussrv3 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "Otussrv3",
        :net => [
#                   ["192.168.255.10",  2,  "255.255.255.252",  "office1-central"],
#                   ["192.168.2.1",     3,  "255.255.255.192",  "dev1-net"],
#                   ["192.168.2.65",    4,  "255.255.255.192",  "test1-net"],
#                   ["192.168.2.129",   5,  "255.255.255.192",  "managers-net"],
#                   ["192.168.2.193",   6,  "255.255.255.192",  "office1-net"],
                   ["192.168.50.13",   8,  "255.255.255.0"],
                ]
  },

  :Otussrv4 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "Otussrv4",
        :net => [
 #                  ["192.168.2.130",  2,  "255.255.255.192",  "managers-net"],
                   ["192.168.50.14",  8,  "255.255.255.0"],
                ]
  },

  :Otussrv5 => {
       :box_name => "generic/ubuntu2204",
       :vm_name => "Otssrv5",
       :net => [
#                   ["192.168.255.6",  2,  "255.255.255.252",  "office2-central"],
#                   ["192.168.1.1",    3,  "255.255.255.128",  "dev2-net"],
#                   ["192.168.1.129",  4,  "255.255.255.192",  "test2-net"],
#                   ["192.168.1.193",  5,  "255.255.255.192",  "office2-net"],
                   ["192.168.50.15",  8,  "255.255.255.0"],
               ]
  },

  :Otussrv6 => {
       :box_name => "generic/ubuntu2204",
       :vm_name => "Otussrv6",
       :net => [
#                  ["192.168.1.2",    2,  "255.255.255.128",  "dev2-net"],
                  ["192.168.50.16",  8,  "255.255.255.0"],
               ]
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      
      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
      end

      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
      SHELL
    end
  end
end
