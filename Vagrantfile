# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  servers =[
    {
      :hostname => "Ubuntu",
      :box => "bento/ubuntu-18.04",
      :ip => "172.16.1.50",
      :ssh_port => '2200'
    },
    {
      :hostname => "Mint",
      :box => "npalm/mint17-amd64-cinnamon",
      :ip => "172.16.1.51",
      :ssh_port => '2201'
    },
    {
      :hostname => "Centos",
      :box => "centos/7",
      :ip => "172.16.1.52",
      :ssh_port => '2202'
    },
  ]

  servers.each do |machine|
    config.vm.define machine [:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.network :private_network, ip: machine[:ip]
            node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id:"ssh"
            
            node.vm.provider "virtualbox" do |vb|
              vb.memory = "2048"
              vb.cpus = "3"
            end
      end
  end
end
  
