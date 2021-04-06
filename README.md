# Cobbler
### Linux PXE Cobbler setup ins.


After resolving any dependicies i.e. dnf module install httpd:2.4/common    
### It is 2021 and I am swimming in RHEL 8 so I will add the following  
dnf install epel-release  
dnf module enable cobbler  
dnf install cobbler  

After resolving any dependicies i.e. dnf module install httpd:2.4/common  

cobbler check - this will list out action items to resolve

#### Installing the web moduel and it has a python3-django conflict so...
dnf install cobbler-web --allowerasing
