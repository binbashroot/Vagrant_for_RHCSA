Vagrant.configure("2") do |config|
config.vm.define "controller" do |controller|
      controller.vm.box = "roboxes/alma8"
      controller.vm.provider :virtualbox do |vb|
             vb.name = "controller"
             vb.customize ["modifyvm", :id, "--memory", "4096"]
             vb.customize ["modifyvm", :id, "--cpus", "2"]
			 vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
         end

      controller.ssh.insert_key = false
	  controller.vm.synced_folder "provision/", "/vagrant/provision"
      controller.vm.provision "ansible_local" do |ansible|
         ansible.playbook = "provision/controller.yml"
         ansible.install_mode ="pip3"
         ansible.verbose = true
         end
end

config.vm.define "servera" do |servera|
      servera.vm.box = "roboxes/rocky8"
      servera.vm.provider :virtualbox do |vb|
             vb.name = "servera"
             vb.customize ["modifyvm", :id, "--memory", "4096"]
             vb.customize ["modifyvm", :id, "--cpus", "2"]
			 vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
         end
      servera.vm.synced_folder "provision/", "/vagrant/provision" 
      servera.ssh.insert_key = false
      servera.vm.provision "ansible_local" do |ansible|
         ansible.playbook = "provision/clients.yml"
         ansible.install_mode ="pip3"
         ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
          end
    end

config.vm.define "serverb" do |serverb|
      serverb.vm.box = "roboxes/rhel7"
      serverb.vm.provider :virtualbox do |vb|
             vb.name = "serverb"
             vb.customize ["modifyvm", :id, "--memory", "4096"]
             vb.customize ["modifyvm", :id, "--cpus", "2"]
 		     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
         end
      serverb.trigger.before :destroy do |trigger|
         trigger.info = "Unregistering from Red Hat subscription"
         trigger.run_remote = {inline: "sudo subscription-manager unregister"}
         end
	  serverb.vm.synced_folder "provision/", "/vagrant/provision"
	  serverb.ssh.insert_key = false
	  serverb.vm.provision "shell", inline: <<-SHELL
	     sudo subscription-manager register --username='YOUR_REDHAT_LOGIN_ID' --password='YOUR_REDHAT_LOGIN_PASSWORD'
SHELL
      serverb.vm.provision "ansible_local" do |ansible|
         ansible.playbook = "provision/clients.yml"
         end
	  
	  serverb.vm.provision "shell",
		  reboot: true,
		  inline:  'echo  Rebooting'
		serverb.vm.provision "shell",
		  inline:  'echo Reboot Completed'
	end

config.vm.define "serverc" do |serverc|
      serverc.vm.box = "roboxes/rhel8"
      serverc.vm.provider :virtualbox do |vb|
             vb.name = "serverc"
             vb.customize ["modifyvm", :id, "--memory", "4096"]
             vb.customize ["modifyvm", :id, "--cpus", "2"]
 		     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
         end
      serverc.trigger.before :destroy do |trigger|
         trigger.info = "Unregistering from Red Hat subscription"
         trigger.run_remote = {inline: "sudo subscription-manager unregister"}
         end
	  serverc.vm.synced_folder "provision/", "/vagrant/provision"
	  serverc.ssh.insert_key = false
	  serverc.vm.provision "shell", inline: <<-SHELL
	     sudo subscription-manager register --username='YOUR_REDHAT_LOGIN_ID' --password='YOUR_REDHAT_LOGIN_PASSWORD'
SHELL
      serverc.vm.provision "ansible_local" do |ansible|
         ansible.playbook = "provision/clients.yml"
         ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
         end
	  
	  serverc.vm.provision "shell",
		  reboot: true,
		  inline:  'echo  Rebooting'
	  serverc.vm.provision "shell",
		  inline:  'echo Reboot Completed'
	end
	
end
