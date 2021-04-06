# Cobbler
### Linux PXE Cobbler setup ins.


After resolving any dependicies i.e. dnf module install httpd:2.4/common    
#### It is 2021 and I am swimming in RHEL 8 so I will add the following  
dnf install epel-release  
dnf module enable cobbler  
dnf install cobbler  

After resolving any dependicies i.e. dnf module install httpd:2.4/common  

cobbler check - this will list out action items to resolve

#### Installing the web moduel and it has a python3-django conflict so...
dnf install cobbler-web --allowerasing

#### How I edited /etc/cobbler/dhcp.templates

subnet 10.0.0.0 netmask 255.255.255.0 {  
&nbsp;&nbsp; option routers             10.0.0.1;  
&nbsp;&nbsp; option domain-name-servers 10.0.0.1;  
&nbsp;&nbsp; option subnet-mask         255.255.255.0;  
&nbsp;&nbsp; range dynamic-bootp        10.0.0.30 10.0.0.254;  
&nbsp;&nbsp; default-lease-time         21600;  
&nbsp;&nbsp; max-lease-time             43200;  
&nbsp;&nbsp; next-server                $next_server;  
