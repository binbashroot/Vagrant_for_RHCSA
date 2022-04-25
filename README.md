Vagrant_for_RHCSA
------------------
The following instructions are how I set up for RHCSA,RHCE, and Ansible testing in my home lab.  
The instructions were written for use in Windows environment, but can be adjusted accordingly for Linux.  
Keep in mind that I have a "Controller" server for Ansible related tasks across the full lab.  
Your Controller server should be configured to provide additional core services for your other servers.  
The configuration of the lab services is outside the scope of this document. This document is strictly  
related to the initial configuration of lab build out.  

### KEY NOTES ###
Vagrant VMs use 2CPUS and 4G of RAM per VM  
Disk size for the servers is set as a default by the robox vm.  
Controller.yml configures the controller server to be a dhcp/tftp server.  With additonal configuration it can be a kickstart server.  
Controller.yml also creates a basic Ansible inventory for testing with the 3 clients.  
clients.yml configures the clients to use eth1 with a static IP for testing.
All vagrant boxes use defined ssh ports for host to guest access. 
All vagrant boxes use defined IPs. 

  
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

7. **Clone the Vagrant_for_RHCSA files from GitHub into your Hashicorp dir**  

    *Windows Powershell (as Adminstrator)*
    ```
    cd C:\Hashicorp\Vagrant\
    git clone git@github.com:binbashroot/Vagrant_for_RHCSA.git
    cp -r Vagrant_for_RHCSA/* .
    rm -r -fo Vagrant_for_RHCSA 
    ```
    
    *Linux (become root as needed)*
    ```
     cd /opt/vagrant
     git clone git@github.com:binbashroot/Vagrant_for_RHCSA.git
     cp -r Vagrant_for_RHCSA/* .
     rm -rf Vagrant_for_RHCSA
    ```
  
8. **Set RHel Subscription User/PWD**  
    Export two environment variables to setup the subscription manager to use credentials:

    ```
    Windows Powershell:
    $env:RHSM_USER = "user@redhat.com"
    $env:RHSM_PASS = "passwordforuser"

    Linux:
    export RHSM_USER=user@redhat.com
    export RHSM_PASS=passwordforuser
    ```
      
9. **Start vagrant**  
     **##### Linux #####**  
     (You may have to become root)  
     **Command:**  vagrant up   
     
     
     **##### Windows #####**  
     (You may need to run as Administrator)  
     **Command:**  vagrant up
       
    
