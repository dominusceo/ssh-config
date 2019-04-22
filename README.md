# Explanation
This is my personal configuration for ssh_config file client and one example of use.
# Goal
This repo represent the personalisation of ssh connections and one example of its usability into an organizational environment. 

# Considerations
This manual applies only for \*nix/linux systems.

# Prerequisites
To bring about this manual, you must consider:

	1. Configure pertinent aliases into a client machine (into your client.example.com)
	2. Download all software or necessary packages
	   - Nagstamon, status monitor for the desktop [Nagstamon Official page](https://nagstamon.ifw-dresden.de) or throught your packager management of your linux distro.

# Topology
![SSH Topology](https://drive.google.com/uc?id=1mLfHoNv2qfURZyaSqtxBMbakpbDEBHLT "Title")


# Manual
1. Install Nagstamon software according with your linux distribution (in my case was for Fedora 29):

```shell
~]# dnf install -y https://nagstamon.ifw-dresden.de/files/unstable/nagstamon-3.1.99rc2.fedora29-1.noarch.rpm
```
or
```shell
~]# dnf install -y nagstamon.noarch
```
2. Create alias in your .bash_profile client 
   - On \*nix/linux terminal client, into config file shell environment create, in this case into .bash_profile:
   ```
   aliases admon="ssh ricardo.carrillo@proxy-ssh-server.example.com"
   ```
3. Create config file into path $HOME/.ssh/
   - Add this configuration and chage according to your data
   ```shell
   # Autor: Ricardo D. Carrillo Sanchez
   # Defining host principal host connection (proxy ssh server) to access trought this
   # to any host of an organistation
   StrictHostKeyChecking no
   Host proxy-ssh-server.example.com
   # Useful aliase (must be defined prevously)
   Host admon
   User ricardo.carrillo
   HostName proxy-ssh-server.example.com
   IdentityFile ~/.ssh/id_rsa

   # Hosts from network 192
   Host 192.168.*
   User admin
   Port 22
   ProxyCommand ssh admon nc %h %p

   # Hosts from networks 10
   Host 10.1.*
   User admin
   Port 22
   ProxyCommand ssh admon nc %h %p
   ```
   - Change permissions
   ```shell
   ~]# cd $HOME/.ssh/ && chmod 644 config
   ```

4. Test your changes (Setting nagstamon):
   - Configure de Nagios server in nagstamon:
     ![Set nagios server](https://drive.google.com/uc?id=1mLfHoNv2qfURZyaSqtxBMbakpbDEBHLT "Nagios Server")
   - Configure nagstamon ssh tunneling
     ![Set ssh settings](https://drive.google.com/uc?id=1mLfHoNv2qfURZyaSqtxBMbakpbDEBHLT "Setting ssh options for tunneling")
   - Test your changes - Test the connection
     ![Test Ssh connection](https://drive.google.com/uc?id=1mLfHoNv2qfURZyaSqtxBMbakpbDEBHLT "Test Ssh tunneling")
    

