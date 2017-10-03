
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = false

  config.vm.define :web_server1 do |wb1|
    wb1.vm.box = "centos1706_v0.2.0"
    wb1.vm.network :private_network, ip: "192.168.56.101"
    #wb.vm.network "public_network", bridge: "eth4", ip:"192.168.131.100", netmask: "255.255.255.0"
    wb1.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024","--cpus", "1", "--name", "web_server1" ]
    end
    config.vm.provision :chef_solo do |chef|
      chef.install = false
      chef.cookbooks_path = "cookbooks"
      chef.add_recipe "httpd"
      chef.add_recipe "haproxy"
      chef.json = {"courses":[
        {"id":"001", "name":"Sistemas distribuídos-s1" },
        {"id":"002", "name":"Redes convergentes-s1" },
        {"id":"003", "name":"Computación en la nube-s1" },
        {"id":"004", "name":"Sistemas operativos-s1" }
      ]}
    end
  end

  config.vm.define :web_server2 do |wb2|
    wb2.vm.box = "centos1706_v0.2.0"
    wb2.vm.network :private_network, ip: "192.168.56.103"
    wb2.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024","--cpus", "1", "--name", "web_server2" ]
    end
    config.vm.provision :chef_solo do |chef|
      chef.install = false
      chef.cookbooks_path = "cookbooks"
      chef.add_recipe "httpd"
      chef.json = {"courses":[
        {"id":"001", "name":"Sistemas distribuídos-s2" },
        {"id":"002", "name":"Redes convergentes-s2" },
        {"id":"003", "name":"Computación en la nube-s2" },
        {"id":"004", "name":"Sistemas operativos-s2" }
      ]}

    end
  end

  config.vm.define :db_server do |db|
    db.vm.box = "centos1706_v0.2.0"
    db.vm.network :private_network, ip: "192.168.56.102"
    db.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024","--cpus", "1", "--name", "db_server" ]
    end
    db.vm.provision :chef_solo do |chef|
      chef.install = false
      chef.cookbooks_path = "cookbooks"
      chef.add_recipe "mysql"
    end
 end


 config.vm.define :balanceador do |hp|
   hp.vm.box = "centos1706_v0.2.0"
   hp.vm.network :private_network, ip: "192.168.56.104"
   hp.vm.provider :virtualbox do |vb|
     vb.customize ["modifyvm", :id, "--memory", "1024","--cpus", "1", "--name", "balanceador" ]
   end
   hp.vm.provision :chef_solo do |chef|
     chef.install = false
     chef.cookbooks_path = "cookbooks"
     chef.add_recipe "haproxy"
     chef.json = {
      "web_servers" => [
         {"ip":"192.168.56.101"},
         {"ip":"192.168.56.103"}
     ]}
   end
 end

end
