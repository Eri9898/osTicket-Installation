<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Installation</h1>
This tutorial outlines the installation of the open-source help desk ticketing system osTicket.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)


<h2>Installation Steps</h2>

<p>
<img src="https://github.com/Eri9898/osticket-prereqs/assets/143845247/ab671f05-1dfe-4c06-a4a0-c781f6bcabee.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

1. To begin you must first create a resource group within Microsoft Azure.  Then create a Windows10 Machine with 2-4 CPUs, make sure it is within the resource group you created. Allow the machine to create a virtual network and subnet.
*   Since this machine will be hosting a server web application extra secuirty precautiomns will be in place

</p>
<p>

</p>
<br />

<p>
<img src="https://github.com/Eri9898/osticket-prereqs/assets/143845247/261ff49c-4e4f-494c-8d2b-b1eefb5ea4b4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
2. Connect to the VM via Remote Desktop and within the VM enable IIS. 
 ***This step turns the VM into a web server essentially. Ports 443 and 80 must also allow inbound traffic otherwise the web service wiill be invisible to the world. 
Go to the control panel, then click Programs, control Panel>programs>Programs & Features> Windows Features On/Off, then scroll down and check the IIS box. Make sure the last 2 boxes are checked.
</p>
<br />
3. Next you must enable CGI and common HTTP Features. On the same windows feaures list scroll to world wide services>App Development> CGI and check the box. Scroll further down the list, check  and expand HTTP Features.  Make sure all the boxes within HTTP features are turned on. 
***IIS needs CGI and HTTP features in order to host dynamic and static content  
</p>
<br />
4. Make sure IIS Managment Console is checked. Access it by scrolling back up to IIS on the same windows features window,  clicking on IIS>Web Management Tools>IIS Mangment Console
***this is the Graphic User Interface for osTicket, which allows for efficient management of web applications without relying soley on code-based tools like powershell or python. IIS manager also includes built-in diagnostic features that are valauble for real world trouble shooting.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/GwE2WGF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
5. Hit Ok and the computer should install IIS. On the web browser look up the local host 127.0.0.1 and the webpage should load. (it may not load on microsoft edge, if that is the case then download google chrome or another browser. Then search up the local host on there.)
*** If the IIS page displays then it means the VM has succesfully connected to it's own web server and can serve web content. This also confirms Port 80 is open allowing the server to recieve and respond to HTTP requsts. 
If HTTPS is configured with a valid certficate you can also confirm port 443 is open and cn handle encrypted traffic.
<br />
6. Next you must install a PHP Manager for IIS (PHPManagerForIIS_V1.5.0.msi). Download the file then Open the file to install it.
https://drive.google.com/file/d/1RHsNd4eWIOwaNpj3JW4vzzmzNUH86wY_/view?usp=share_link
*** PHP manager for IIS provides a GUI to configure PHP based web apps like osTicket. PHP support can be enabled on IIS without needing to be manually edit comnfig files or use command line tools.
</p>
<br />
7. Follow the same procedure for downloading a rewrite module (rewrite_amd64_en-US.msi). 
https://drive.google.com/file/d/1tIK9GZBKj1JyUP87eewxgdNqn9pZmVmY/view?usp=share_link
*** rewrite modules help organize and secure web traffic and more. 
</p>
<br />
8. Next install PHP (php-7.3.8-nts-Win32-VC15-x86.zip) https://drive.google.com/file/d/1snNMtLdCOpMtkCyD4mvl9yOOmvVIp9fP/view?usp=share_link
*** osTicket is a PHP based app, so installing PHP helps IIS to properly process the code within osTicket's files. Without this IIS can't understand or run osTicket basically.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/yA0dnbM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
9. Create a new file named "PHP" for PHP on the local harddrive., unzip the PHP contents into C:/PHP
*** Unzipping the PHP file into a folder on the local drive (C:\PHP) gives IIS a static, reliable location to find the PHP executables and configuration files. This setup keeps everything organized and is important for properly linking PHP to FastCGI, which is how IIS processes PHP scripts efficiently. FastCGI basically translate's osTicket's PHP codes so that IIS understands them and can execute.
</p>
<br />
10. Next download Microsoft Visual C + + (VC_redist.x86.exe.) 
https://drive.google.com/file/d/1s1OsGF3-ioO0_9LYizPRiVuIkb3lFJgH/view?usp=share_link
xxxInstalling the Microsoft Visual C++ Redistributable gives PHP the runtime libraries it needs to work correctly.
These libraries include pre-written code for things like file handling and memory use.
Since PHP was built using C++, it expects these resources to be there. Without them, PHP (and apps like osTicket that rely on it) can crash or fail to run properly.
</p>
<br />
11.  Next Install a database ((mysql-5.5.62-win32.msi) https://drive.google.com/file/d/1_OWh9p7VQLcrB0q_V7qT8yHl0xo5gv7z/view?usp=share_link
Select the typical install, click finish to launch the configuariation wizard. Within that check standard configuaration and install.
Create your credentials for the database, your username will automatically be “root”. So create your password.  Do not forget it! Then execute.
xxxInstalling my sql gives os ticket a database to store, retrieve and manage all its info like tickets, users, pretty much everything in osticket. 
</p>
<br />
12. Next search IIS on the windows start menu, right click and open as admin.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/s5jUSDP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
13. Open the PHP manager that was installed earlier. Click register and it will ask for a location, click the three dots. Locate file C:\PHP\CGI and select it.
</p>
<br />
</p>
<br />
<img src="https://imgur.com/5AHq3tk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
14. Refresh IIS AFTER selecting the file.
 Then download OSTicket (osTicket v 1.15.8) https://drive.google.com/file/d/13IsQ-fzu5Nms5LSkfaiIpwvEoa-Jc75z/view?usp=drive_link onto the machine. 
Open the OSTicket file, extract and copy the zipped file "Upload" within it. Copy it into c:\inetpub\wwwroot
</p>
<br />
15. Within c:\inetpub\wwwroot rename the upload file to "OSTicket"
</p>
<br />
</p>
<br />
<img src="https://imgur.com/fMTpCKA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
16. Reload IIS again and in the upper left corner go click on Ticket\YourName> Sites> Default> osTicket. On the right side of the screen click “Browse *:80” and the welcome webpage to IIS should load. It says “Thanks for choosing osTicket!”
</p>
<br />
17. On that same page, Ticket\YourName> Sites> Default> osTicket.  Double-click PHP Manager within OSticket. Click “Enable or disable an extension”
Enable: php_imap.dll
Enable: php_intl.dll
Enable: php_opcache.dll
Then Refresh osTicket Browser
</p>
<br />
18. Rename ost-sampleconfig.php to ost-config.php
It is located in
C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php
</p>
<br />
</p>
<br />
<img src="https://imgur.com/t2GHOsE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
19. Next you will assign permissions to ost-config.php.  Right click to properties and go to security. Click advanced then disable inheritance, click remove all permissions. Next click add, click select principle and type everyone and click ok. Check box “full control” and apply.
</p>
<br />
20. Instal HeidiSQL so that you can connect to MySQL. https://docs.google.com/document/d/1WovrX2DaS9xkfaSr4LXyB4YnnWpXIgPCMMbbfgHmGVw/edit
Open the file and install it. The app shoud open after install. On the bottom left click new, then enter username:Root then password:
Create a new session, enter username and passowrd.
</p>
<br />
21. Create a new session and connect to it by clicking open.  
</p>
<br />
</p>
<br />
<img src="https://imgur.com/Tcf1nhi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
22. In Heidi right click unnamed in upper left corner, then click create new then database.then name it osTIcket then say ok
</p>
<br />
</p>
<br />
<img src="https://imgur.com/GPlKJnb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
23. Continue Setting up osTicket in the ISS webpage, click next. Next fill out the system settings and in database settings enter your MY SQL 
MySQL Table Prefix: ost-config.php
MySQL Database: osTicket
MySQL Username: root
MySQL Password: ***** whatever you created
Click “Install Now!”
</p>
<br />
24. OS TIcket should then load up with a welcome screen. Congratulations you just installed os Ticket! c: 
