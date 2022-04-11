# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Setup subscription manager using environment variables to login with
# This elimates hard coding passwords in the code
user = ENV['RH_SUBSCRIPTION_MANAGER_USER']
password = ENV['RH_SUBSCRIPTION_MANAGER_PW']
if !user or !password
  puts 'Required environment variables not found. Please set RH_SUBSCRIPTION_MANAGER_USER and RH_SUBSCRIPTION_MANAGER_PW'
  abort
end

register_script = %{
if ! subscription-manager status; then
  sudo subscription-manager register --username=#{user} --password=#{password} --auto-attach
fi
}

unregister_script = %{
if subscription-manager status; then
  sudo subscription-manager unregister
fi
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

   # if vagrant-vbguest is installed stop auto updating virtualbox guest add-on update
   if defined? VagrantVbguest
   config.vbguest.auto_update = false
   end

   ENV["LC_ALL"] = "en_US.UTF-8"
   ENV['LANG']="en_US.UTF-8" 

   config.vm.define "controller" do |controller|
      controller.vm.box = "roboxes/alma8"
      controller.vm.network "private_network", ip: "192.168.80.10",
         virtualbox__intnet: "ClientNetwork"
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
      servera.vm.network "private_network", ip: "192.168.80.21",
         virtualbox__intnet: "ClientNetwork"
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
      serverb.vm.network "private_network", ip: "192.168.80.22",
         virtualbox__intnet: "ClientNetwork"
      serverb.vm.provider :virtualbox do |vb|
         vb.name = "serverb"
         vb.customize ["modifyvm", :id, "--memory", "4096"]
         vb.customize ["modifyvm", :id, "--cpus", "2"]
 		   vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
         vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
      end
      serverb.trigger.before :destroy do |trigger|
         trigger.info = "Unregistering from Red Hat subscription"
         trigger.run_remote = {inline: unregister_script}
      end
      serverb.vm.synced_folder "provision/", "/vagrant/provision"
      serverb.ssh.insert_key = false
      serverb.vm.provision "shell", inline: register_script
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
      serverc.vm.network "private_network", ip: "192.168.80.23",
         virtualbox__intnet: "ClientNetwork"
      serverc.vm.provider :virtualbox do |vb|
         vb.name = "serverc"
         vb.customize ["modifyvm", :id, "--memory", "4096"]
         vb.customize ["modifyvm", :id, "--cpus", "2"]
         vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
         vb.customize ["modifyvm", :id, "--nic2", "natnetwork", "--nat-network2", "ClientNetwork"]
      end
      serverc.trigger.before :destroy do |trigger|
         trigger.info = "Unregistering from Red Hat subscription"
         trigger.run_remote = {inline: unregister_script}
      end
      serverc.vm.synced_folder "provision/", "/vagrant/provision"
      serverc.ssh.insert_key = false
      serverc.vm.provision "shell", inline: register_script
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
