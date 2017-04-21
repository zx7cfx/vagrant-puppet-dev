# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
require 'yaml'

servers = YAML.load_file('servers.yaml')

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	servers.each do |servers|
		config.vm.define servers["name"], autostart: servers["autostart"] do |srv|
			srv.vm.box = servers["box"]
			srv.vm.hostname = servers["name"]
			srv.vm.box_url = servers["box_url"]
			srv.vm.network :private_network, ip: servers["ip"]
 			srv.vm.provider :virtualbox do |v|
				v.customize ["modifyvm", :id, "--memory", servers["ram"]]
 				v.customize ["modifyvm", :id, "--name", servers["name"]]
			end
	 		srv.vm.provision :hosts, :sync_hosts => true
	 		srv.vbguest.auto_update = false
	 		srv.ssh.insert_key = false
			srv.vm.provision "file", source: servers["public_key"], destination: "~/.ssh/authorized_keys"
		end
	end
end