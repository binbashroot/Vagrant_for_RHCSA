Vagrant_for_RHCSA
------------------
The following instructions are how I set up for RHCSA,RHCE, and Ansible testing in my home lab.  
The instructions were written for use in Windows environment, but can be adjusted accordingly for Linux.  
Keep in mind that I have a "Controller" server for Ansible related tasks across the full lab.  
Your Controller server should be configured to provide core services for your other servers.  
The configuration of the lab services is outside the scope of this document. This document is strictly  
related to the initial configuration of a lab.  

### KEY NOTES ###
Vagrant VMs use 2CPUS and 4G of RAM per VM  
Disk size for the servers is set as a default by the Vagrantfile.  
  
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

7. **Download the Vagrantfile.**  
    Do not change the name.  

8. **Move the Vagrantfile to the Vagrant homedir**  
    Windows: C:\Hashicorp\Vagrant\Vagrantfile  
    Linux:  /opt/vagrant/Vagrantfile  
    
9. **Edit the Vagrantfile**  
    Update "serverb" and "serverc" with your redhat login credentials.  **Be aware........your credentials will be in plain text**
    You can opt out of this by removing the following lines from the vagrantfile for both serverb AND serverc.  
   **Example for serverb:**  
 ```
    serverb.vm.provision "shell", inline: <<-SHELL  
	 sudo subscription-manager register --username='YOUR_REDHAT_LOGIN_ID' --password='YOUR_REDHAT_LOGIN_PASSWORD'  
    SHELL
 ```  
 10. **Upload Ansible Playbooks**  
     Create an Ansible playbook called:  controller.yml  
     Create an Ansible playbook called:  clients.yml
 12. **Start vagrant**  
     ##### Linux #####  
     Open a terminal  
     Become root if necessary   
     vagrant up  
     ##### Windows #####  
     Open a Powershell window as Adminstrator  
     vagrant up
       
    
