# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/ubuntu2004"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 4096
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # ELK server.
  config.vm.define "logs" do |logs|
    logs.vm.hostname = "logs.test"
    logs.vm.network :private_network, ip: "10.108.9.90"

    logs.vm.provision :ansible do |ansible|
      ansible.playbook = "main.yml"
      ansible.inventory_path = "inventory/inventory"
      ansible.become = true
    end
  end

  # random webserver for practice to forward logs to elk
  config.vm.define "web" do |web|
    web.vm.hostname = "web.test"
    web.vm.network :private_network, ip: "10.108.9.91"

    web.vm.provider :virtualbox do |v|
      v.memory = 512
      v.cpus = 1
    end

    web.vm.provision :ansible do |ansible|
      ansible.playbook = "web/main.yml"
      ansible.inventory_path = "inventory/inventory"
      ansible.become = true
    end
  end

end
