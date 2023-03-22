# How to install a local GitLab repository
Go to https://www.osboxes.org/virtualbox-images/ choose CentOS  osboxes osboxes.org

Start the virtual machine 
Change CPU for 2 and create network 
1 - Bridge Adapter - Wireless
2 - Host Only

Clone the virtual machine - Linked Clone
Create a static network config
Take the ip is already defined. Check it ou by running ip a command.
vi /etc/sysconfig/network-scripts/ifcfg-enp0s8


DEVICE=enp0s8
IPADDR=192.168.56.102
NETMASK=255.255.255.0
NETWORK=192.168.56.0
# If you're having problems with gated making 127.0.0.0/8 a martian,
# you can change this to something else (255.255.255.255, for example)
BROADCAST=192.168.56.255
ONBOOT=yes
NAME=enp0s8
