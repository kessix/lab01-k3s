# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "manager" => {"memory" => "1024", "cpu" => "1", "ip" => "10", "image" => "centos/8"},
  "worker01" => {"memory" => "512", "cpu" => "1", "ip" => "11", "image" => "centos/8"},
  "worker02" => {"memory" => "512", "cpu" => "1", "ip" => "12", "image" => "centos/8"} 
}

Vagrant.configure("2") do |config|

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}.k3scluster.local"
      machine.vm.network "private_network", ip: "172.1.0.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/k3s-cluster"]
      end
      #machine.vm.provision "shell", path: "provision.sh"
      machine.vm.provision "shell", inline: "hostnamectl set-hostname #{name}.k3scluster.local"
    end
  end
end

