Vagrant_for_RHCSA
------------------
The following instructions are how I set up for RHCSA,RHCE, and Ansible testing in my home lab.  
The instructions were written for use in Windows environment, but can be adjusted accordingly for Linux.  
Keep in mind that I have a "Controller" server for Ansible related tasks across the full lab.  
Your Controller server should be configured to provide additionalo core services for your other servers.  
The configuration of the lab services is outside the scope of this document. This document is strictly  
related to the initial configuration of lab build out.  

### KEY NOTES ###
Vagrant VMs use 2CPUS and 4G of RAM per VM  
Disk size for the servers is set as a default by the Vagrantfile.  
Controller.yml configures the controller server to be a dhcp/tftp server.  With additonal configuration it can be a kickstart server.  
Controller.yml also creates a basic Ansible inventory for testing with the 3 clients.  
clients.yml configures the clients to use eth1 with a static IP for testing.
  
### Instructions ###


1. **Download and install Virtualbox** https://www.virtualbox.org/wiki/Downloads
2. **Download and install Virtualbox extensions** https://www.virtualbox.org/wiki/Downloads
3. **Download and install Vagrant (Hashicorp)** https://www.vagrantup.com/downloads
4. **Start VirtualBox and create a new "NatNetwork"**  
File > Preferences > Network > New Network (this will create a new NatNetwork  

5. **Modify "NatNetwork"**  
   Double click "NatNetwork"    
   Change Network Name to "ClientNetwork"   
   Change Network CIDR to 192.168.80.0/24  
   Leave "Support DHCP" and "Supports IPv6" unchecked  
   Click OK  

6. **Make a provision directory.**  
    This directory should reside inside your Vagrant installation directory.   
    This is where you will put your playbooks for the Ansible provisioner in Vagrant.   
    Windows: mkdir C:\Hashicorp\Vagrant\provision  
    Linux:   mkdir /opt/vagrant/provision  

7. **Download files from GitHub**  
    Note: Do not change the filenames  
    - Vagrantfile  
    - controller.yml  
    - clients.yml    
    

8. **Move Downloaded Files to proper location**    

    **##### clients.yml #####**  
    - Windows: C:\Hashicorp\Vagrant\Vagrantfile\provision\clients.yml
    - Linux:  /opt/vagrant/Vagrantfile/provision/clients.yml   
 
    **##### controller.yml #####**    
    - Windows: C:\Hashicorp\Vagrant\Vagrantfile\provision\controller.yml
    - Linux:  /opt/vagrant/Vagrantfile/provision/controller.yml  
  
    **##### Vagrantfile #####**  
    - Windows: C:\Hashicorp\Vagrant\Vagrantfile    
    - Linux:  /opt/vagrant/Vagrantfile  
    
9. **Edit the Vagrantfile**  
    Update "serverb" and "serverc" with your redhat login credentials.  **Be aware........your credentials will be in plain text**  
    You can opt out of this by removing the following lines from the vagrantfile for both serverb AND serverc.  
  
 
 ```
  ####Example for serverb:####  
    serverb.vm.provision "shell", inline: <<-SHELL  
	 sudo subscription-manager register --username='YOUR_REDHAT_LOGIN_ID' --password='YOUR_REDHAT_LOGIN_PASSWORD'  
    SHELL
 ```  
      
 10. **Start vagrant**  
     **##### Linux #####**  
     Open a terminal  (You may have to become root)  
     **Command:**  vagrant up   
     
     
     **##### Windows #####**  
     Open a Powershell window as Adminstrator   
     **Command:**  vagrant up
       
    
