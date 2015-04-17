# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.hostname = "ansible.local"
    config.vm.network :private_network, ip: "33.33.33.42"

    config.vm.provider :virtualbox do |vb|
        config.vm.box = "precise64"
        config.vm.box_url = "http://files.vagrantup.com/precise64.box"

        # Properly configure the vm to use the available amount of cores
        vb.customize ["modifyvm", :id, "--cpus", `#{RbConfig::CONFIG['host_os'] =~ /darwin/ ? 'sysctl -n hw.ncpu' : 'nproc'}`.chomp]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]

        vb.customize ["modifyvm", :id, "--memory", 2048]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.inventory_path = "inventory/vagrant"
        ansible.playbook = "provision.yml"
        ansible.limit = "vagrant"
        ansible.verbose = false
    end

    config.vm.synced_folder "./", "/home/vagrant/ansible"
end
