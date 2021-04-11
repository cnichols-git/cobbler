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


Issues:

[root@automate www]# cobbler import --name=centos8Stream --arch=x86_64 --path=/home/chris/Storage/Software/ISOs/CentOS-Stream-8-x86_64-20210316-dvd1.iso  
task started: 2021-04-11_111808_import  
task started (id=Media import, time=Sun Apr 11 11:18:08 2021)  
running python triggers from /var/lib/cobbler/triggers/task/import/pre/*  
running shell triggers from /var/lib/cobbler/triggers/task/import/pre/*  
shell triggers finished successfully  
Exception occured: <class 'cobbler.cexceptions.CX'>  
Exception value: 'Command failed'  
Exception Info:  
  File "/usr/lib/python3.6/site-packages/cobbler/remote.py", line 98, in runrc = self._run(self)  

  File "/usr/lib/python3.6/site-packages/cobbler/remote.py", line 305, in runnerself.logger  

  File "/usr/lib/python3.6/site-packages/cobbler/api.py", line 1459, in import_treeutils.run_this(rsync_cmd, (spacer, mirror_url, path), self.logger)  

  File "/usr/lib/python3.6/site-packages/cobbler/utils.py", line 931, in run_thisdie(logger, "Command failed")  

  File "/usr/lib/python3.6/site-packages/cobbler/utils.py", line 108, in dieraise CX(msg)  

!!! TASK FAILED !!!  
