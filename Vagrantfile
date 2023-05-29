# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "generic/debian10",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                ],
	:config => "inetRouter.yml"
  },
  :centralRouter => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-central"},
                   {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hw-central"},
                   {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "mgt-central"},
                   {ip: '192.168.255.5', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "toOffice1"},
                   {ip: '192.168.255.9', adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "toOffice2"},
                ],
	:config => "centralRouter.yml"
  },
  :office1Router => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.2.1', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "dev-office1"},
                   {ip: '192.168.2.65', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "test-office1"},
                   {ip: '192.168.2.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "manager-office1"},
                   {ip: '192.168.2.192', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "hw-office1"},
                   {ip: '192.168.255.6', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "toOffice1"},
                ],
	:config => "office1Router.yml"
  },
  :office2Router => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.1.1', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev-office2"},
                   {ip: '192.168.1.129', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "test-office2"},
                   {ip: '192.168.1.193', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "hw-office2"},
                   {ip: '192.168.255.10', adapter: 5, netmask: "255.255.255.252", virtualbox__intnet: "toOffice2"},
                ],
	:config => "office2Router.yml"
  },
  
  :centralServer => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-central"},
                ],
	:config => "server.yml"
  },
  :office1Server => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.2.130', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "manager-office1"},
                ],
	:config => "server.yml"
  },
  :office2Server => {
        :box_name => "generic/debian10",
        :net => [
                   {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev-office2"},
                ],
	:config => "server.yml"
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
	
	box.vm.provider "virtualbox" do |v|
        	# Set VM RAM size and CPU count
        	v.memory = "512"
        	v.cpus = "1"
     	 end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end

	if boxconfig[:config].length > 0
      		box.vm.provision "ansible" do |ansible|
	          ansible.playbook = boxconfig[:config]
       		  ansible.verbose = "v"
		  if boxname.to_s.include? "Server"
		    	  ansible.extra_vars = { work_ip: boxconfig[:net][0][:ip]}
		  end
		end
	end
      end
  end
end

