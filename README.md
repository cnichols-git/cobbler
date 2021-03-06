# Cobbler
### Linux PXE Cobbler setup ins.

#### It is 2021 and I am swimming in RHEL 8 so I will add the following  
dnf install epel-release  
dnf module enable cobbler  
dnf install cobbler  
After resolving any dependicies i.e. dnf module install httpd:2.4/common  

#

#### Installing the web moduel and it has a python3-django conflict so...
dnf install cobbler-web --allowerasing

#### How I edited /etc/cobbler/dhcp.templates

subnet 10.0.0.0 netmask 255.255.255.0 {  
&nbsp;&nbsp;&nbsp;&nbsp; option routers             10.0.0.1;  
&nbsp;&nbsp;&nbsp;&nbsp; option domain-name-servers 10.0.0.1;  
&nbsp;&nbsp;&nbsp;&nbsp; option subnet-mask         255.255.255.0;  
&nbsp;&nbsp;&nbsp;&nbsp; range dynamic-bootp        10.0.0.30 10.0.0.254;  
&nbsp;&nbsp;&nbsp;&nbsp; default-lease-time         21600;  
&nbsp;&nbsp;&nbsp;&nbsp; max-lease-time             43200;  
&nbsp;&nbsp;&nbsp;&nbsp; next-server                $next_server;  

#

cobbler check - this will list out action items to resolve  

#

#### Cobbler Web
firewall-cmd --add-port=80/tcp --permanent  
success  
firewall-cmd --add-port=443/tcp --permanent  
success  
firewall-cmd --add-service=dhcp --permanent  
success  
firewall-cmd --add-port=69/tcp --permanent  
success  
firewall-cmd --add-port=69/udp --permanent  
success  
firewall-cmd --add-port=4011/udp --permanent  
success  
firewall-cmd --reload  
success  

#  

https://ip/cobbler_web  
To trouble shoot any issues stop cobbler  
Check in var/log/cobbler/cobbler.log for errors

systemctl stop cobblerd  

Then issue cobblerd -F  

This will run the daemon in foreground mode to confirm there aren't any other errors being generated during startup... said the google search  
#

mount -t iso9660 -o loop,ro /path/to/isos/Fedora-Server-dvd-x86_64-28-1.1.iso /mnt  
How to import:  

cobbler import --name=fedora27 --arch=x86_64 --path=/mnt  


Issues:  

cobbler signature report --name=redhat  
cobbler signature update  
