# apache2_web_server
apache2 web server infos and custom files


How to setup in windows 10:
1. download rar file from 
https://www.apachehaus.com/cgi-bin/download.plx?dli=ad0YwIGMBVzTEV0aldEZCpkVOpkVFVVcatGazR1Z

2. extract files to specific folder (different path must be specified in DocumentRoot variable in step 9):
C:/Apache24   

4. Create Apache web server service (especially so windows can start it automatically):
Open command prompt as administrator and type:
cd C:/Apache24/bin/httpd.exe -k install -n "Apache web server"

Confirm that install does not show errors (theywill be mentioned if any). If it does, fix it before moving on.

5. Open windows 10 services:
windows_key+r -> services.msc

Right-click Apache_web_server in list and click start.
You can also choose if windows will start it automatically in this menu.

6. Open firewall for Apache web server:
In windows search bar, write "Windows defender firewall with advanced security" and select it.
Click on "Inbound rules".
In actions menu on right of screen, select "New rule...".
Select "Port", click next.
Select "TCP", "Specific local ports:", enter the local port (80 by default). Click next.
Select Allow the connection, click next.
Check all boxes (domain, private, public), click next.
Give the name "Apache web server", click finish.

7. Add files and folders to file server:
Go in C:/Apache24/htdocs/ and create a new directory. This new directory can be accessed from a browser at this.is.my.ip:port_number/directory_name
  example: 75.135.42.13:2080/data  (port=2080, directory name=data)
  
8. Create a port forwarding rule in the router (specific to each one...):
windows_key+r -> cmd
enter "ipconfig", check the ip address and copy it in a browser but change last number to 1
example: 192.168.0.34 -> 192.168.0.1

find the port forwarding tab or button or menu and create a rule:
name it, select port_number for the local port (default 80), select TCP or both protocols, select outbound port (default 80), select ip adress of your machine found just above (example 192.168.0.34) and save.

(OPTIONAL) 9. To allow password protected folders and files, edit C:/Apache24/conf/httpd.conf and modify this section:
DocumentRoot "${SRVROOT}/htdocs"
<Directory "${SRVROOT}/htdocs">
  AllowOverride None
...
Replace None with All from above, save file.

Restart Apache web server in services.msc.

In the folder you want to be protected, create 2 text files and change names to .htaccess and .htpassword.
Add in .htaccess:
AuthName C:/Apache24/htdocs/data
AuthUserFile C:/Apache24/htdocs/data/.htpassword
AuthType Basic
Require valid-user

Add in .htpassword:
username:{SHA}2+CP5wEowHe/VUEB/sp3dLThE4o=

(where charlesmarseille is a username and {SHA}2+CP5wEowHe/VUEB/sp3dLThE4o= is a generated secure password from https://hostingcanada.org/htpasswd-generator/).
Save both files and user can log in to server on this directory with username, password AND the URL. The protected folders are not visible in the file tree, they must be GET directly with the right URL. both these files must be present in each folder you want protected, but each subdirectories are also protected.
