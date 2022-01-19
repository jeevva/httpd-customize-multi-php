# httpd-customize-multi-php
This is a custom apache server capable of running multiple php on a single server

Every year technology is always changing, improving and adjusting.
But often at certain times we don't need to follow it because our system is quite mature. But on the other hand there is a new system that we want to install and should not affect the system that has been around for a long time.
Example: If you have an application that runs pretty well on PHP version 5 but my new application is only able to run on version 7 then it's not the old application that we need to update. what we have to do is how to do both well.
This Respository is the solution to the above problems.

-------------------------------------------------------------------------------------------

# Operating System Requirements
The primary Windows platform for running Apache 2.4 is Windows 2000 or later. Always obtain and install the current service pack to avoid operating system bugs.

Apache HTTP Server versions later than 2.2 will not run on any operating system earlier than Windows 2000.

# Run Apache as Service
First Download this repository or you can get apache from https://www.apachelounge.com/download/

Extract on dir : C:/Program Files/Apache Software Foundation/

You can install Apache as a Windows NT service as follows from the command prompt at the Apache bin subdirectory:

httpd.exe -k install

# How to running multiple php
This example running php 5.6 and php 7.4
You can also add other php versions that are needed, follow the steps below.

1. Download php from https://windows.php.net/downloads/releases/archives/
2. Extract on Apache/php/
3. Edit httpd.conf in last line add ScriptAlias like this :
      
       ScriptAlias /php7.4.9 "${SRVROOT}/php/php7.4.9"
       <Directory "${SRVROOT}/php/php7.4.9">
         Options +Indexes +Includes +FollowSymLinks +MultiViews
          AllowOverride All
          Require  all granted
        <Files "php-cgi.exe">
        Require all granted
        </Files>
       </Directory>
      

4. Edit httpd-vhost.conf add Unset envirotment PHP for running another PHP version in single server
           
<VirtualHost *:80>
          
	  <VirtualHost *:80>
		  ServerAdmin itsmapp@opusit.com.sg
		  DocumentRoot "${SRVROOT}/htdocs/"
		  ServerName php7.local

		  <Directory  "${SRVROOT}/htdocs">
		  Options +Indexes +Includes +FollowSymLinks +MultiViews
		  AllowOverride All
		  Require  all granted
		  </Directory>
		      UnsetEnv PHPRC
		  <FilesMatch "\.php$">
		      SetHandler application/x-httpd-php7.4.9
		      Action application/x-httpd-php7.4.9 "/php7.4.9/php-cgi.exe"
		  </FilesMatch>
      	  </VirtualHost>
      
5. Add new DNS on C:\Windows\System32\drivers\etc\.hosts

	127.0.0.1       php7.local
	
	::1             php7.local
    
      
6.Restart your Services
 
    httpd.exe -k restart  
    
 ![MultiPHP-pict](https://user-images.githubusercontent.com/38546311/106383423-6ad03000-63f8-11eb-9313-808e0241c0cd.PNG)

  



# Credit :
-httpd.apache.org

-www.php.net

-Antonius Hasoloan

-Giga Iswanto
