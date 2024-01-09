# Deploy ChaZ3 with Docker compose

## Overview
To deploy ChaZ3 with Docker compose, the following steps can be taken:
1. Get a server
1. Prepare the environment
1. Run docker compose

You can join in at any step, or modify as you wish!

## Get a server
An old computer or just a VM, it doesn't matter.

I run VirtualBox on my desktop and use a CentOS 7 Minimal image ([download](https://www.centos.org/download/#centos-linux-7-2009)).  
The network is set to `Bridged adapter`, so it gets its private IP in my home network. Should even assign a fix address in the router. If you want this to be publicly available, you'll need port forwarding or an IPv6 address. But I use it more like a staging environment, so I did not do any of those.

---
For whatever reason, one of my new VMs came up without network connection. The private IP of the LAN should be visible in  
`ip a`  
If there is no IP address, enter  
`nmcli device status`  
to check the `STATE` of the `Connection`. If it is not connected, check if autoconnection is configured to `yes`:  
`nmcli con show enp0s3 | grep connection.autoconnect`  
If `no`, than reconfigure:  
`nmcli con mod enp0s3 connection.autoconnect yes`  
After that, the VM should get an IP.

---
Also do not forget to **insert** and install Guest additions. (Courtesy of [linuxsimply.com](https://linuxsimply.com/linux-basics/os-installation/virtual-machine/virtualbox-guest-additions-centos/).)
```
yum -y install epel-release
yum update -y
yum install -y dkms gcc kernel-devel-$(uname -r) kernel-headers-$(uname -r) make bzip2 elfutils-libelf-devel perl
export KERN_DIR=/usr/src/kernels/$(uname -r)
mkdir -p /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
sh /mnt/cdrom/VBoxLinuxAdditions.run
shutdown -r now
```

## Prepare the environment
The server will need to be running docker, so this must be installed. But instead of the manual install, let's use Ansible.