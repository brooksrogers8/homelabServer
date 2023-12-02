# Dell poweredge R410 Homelab Setup
 
 11/23/2023
 
 I began by installing boot device onto the server, I chose ubuntu. I chose the latest version (22.04.03) and have yet to have any problems 
 ```shell
https://ubuntu.com/download/server
 ```

 Then I went to Rufus to allow me to download the iso onto my USB

 ```shell
 https://rufus.ie/en/
 ```
Then open up roofus and make sure that the device is selected to the correct usb and in the boot selection tab select the ubuntu iso just downloaded. 

When the roofus download is finished, take it out and plug it into the server along with keyboard and mouse. 

When the server starts up press ``F11`` to enter the UEFI Boot Manager and select the ``Front USB``
then select ``install ubuntu``.

Select your language

# Network connections
If you have an ethernet plugged into your server then your ip address should be detected. This is also a good time to set your static ip. Select edit IPv4 and change the IPv4 Method to ``Manual``

enter your networks subnet, if you're at home you're most likely using a class C address so the CIDR (classless inter-domain routing) is /24 

Then set your static IP address, make sure to use an ip address that is outside of your DHCP range to avoid conflicts with other devices on the netwoork that have been automatically given an address by the router.

So for example 192.168.0.200 in a class C.

Next set your gateway, you can find this by typing ``ipconfig`` on a windows command prompt or ``route -n`` in linux.

For name servers, enter a ip address of the DNS server that we would like our server to use, the server converts a domain name ex: google.com into an IP address ex: 172.217.169.78 that a computer can understand

Leave search domains blank.

# Configure proxy
If on a home network you should probably skip proxy address. But if you are, enter the address

# Configure Ubuntu archive mirror
The mirror address is what is used to keep your server up to date, the default should be fine. 

# Guided sotrage configuration
Make sure to select to use the entire desk.

# Storage configuration
Looking at your devices, if you have free space under ubuntu-vg, you'll want to change the storage around to make sure you use all the space available. 

Under used devices, you'll have a device ``ubuntu-lv`` that uses some amount of storage. Edit it to use the max amount, it will list the max amount on the left hand side.

If you dont do this, youll only use probably about half of your storage disk space, in which youll have to change it later on. 

After making sure everything looks right, click done.

# Profile setup

Enter all the information needed to help you log into the system.

# SSH Setup

Since we are using a headless server and there's no GUI its a good idea to select to install OPENSSH server. then click done

# Featured server snaps

Unless theres something specific you want to install listed, click done.

# Post Ubuntu Installation
After selecting reboot when the installation is finished, log into the server. 

# Security
First make sure your server is up to date 
``` shell
sudo apt update
```

``` shell
sudo apt upgrade
```

After running the update restart the server 

``` shell
sudo reboot
```

# Automation
Then automate updates

``` shell
sudo apt install unattended-upgrades
```
to allow the server to restart itslef to complete the update process
``` shell
sudo apt install update-notifier-common
```

# Checking config

``` shell
cd /etc/apt/apt.conf.d
sudo nano 50unattended-upgrades
```

Check automatic reboot line by pressing `ctrl w` and search automatic-reboot and press enter
It should take you to unnattended upgrade line and remove the // that comment out the line, and change the false to true.

Then check unattended-upgrade below and remove the comment lines `//` and change the false to true.

This tells the server to restart automatically following an unattended-upgrade. 
You can also change the automatic-reboot-time to whatever you'd like. 

Now to check the second config file
``` shell
sudo nano 20auto-upgrades
```
Make sure to check that each of them is set to 1

Finally check if automatic update service is running 

``` shell
sudo systemctl status unattended-upgrades
```
If active and running, then success

# Networking

first enter
 ``` shell
ip a
```

``` shell
sudo nano /etc/netplan/00-isntaller-config.yaml
```
If you configured the static ip earlier, then the addresses should look the same, if you need to change anything then do so now. 

# Remote access (local network)
``` shell
sudo apt install openssh-server
```
Go to your pc, and make sure OpenSSH client is enabled

Then go to your command prompt
``` shell
ssh username@server-ip-address

example: ssh brooks@192.168.0.200
```
Then put in password
And now you should be connected successfully from your pc or laptop. 