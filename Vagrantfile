# Define the Vagrant environment for Ansible 101
#
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

#  # Create the Ansible Command Server
#  config.vm.define :acs do |acs|
#    acs.vm.box = "ubuntu/trusty64"
#    acs.vm.hostname = "acs"
#    acs.vm.network "private_network", ip: "192.168.33.10"
#    acs.vm.provider "virtualbox" do |vb|
#      vb.memory = "256"
#    end
#  end

  #Create a load balancer
  config.vm.define :lb01 do |lb|
      lb.vm.box="ubuntu/trusty64"
      lb.vm.hostname = "lb01"
      lb.vm.network "private_network", ip: "192.168.33.20"
      lb.vm.network "forwarded_port", guest: 80, host: 6080
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = "256"
      end
  end

  # Create some app servers
  (1..2).each do |i|
    config.vm.define "app0#{i}" do |node|
        node.vm.box="ubuntu/trusty64"
        node.vm.hostname = "app0#{i}"
        node.vm.network :private_network, ip: "192.168.33.2#{i}"
        node.vm.network "forwarded_port", guest: 80, host: "608#{i}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "256"
        end
    end
	config.vm.synced_folder ".", "/root/wp-install"
  end

  config.vm.define "db01" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.hostname = "db01"
    db.vm.network "private_network", ip: "192.168.33.50"
    db.vm.network "forwarded_port", guest: 80, host: 8080
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
     end

     config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.limit="all"
        ansible.playbook = "playbook.yml"
        ansible.vault_password_file = "./vault_pass.txt"
   #     ansible.inventory_path = "./hosts"
        ansible.groups = {
  	  "apps" => ["app01", "app02"],
  	  "dbs" => ["db01"],
  	  "lbs" => ["lb01"],
  	  "apps:vars" => {"server_role" => "app"},
  	  "dbs:vars" => {"server_role" => "db"},
  	  "lbs:vars" => {"server_role" => "lb"}
}
  end
end
