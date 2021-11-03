# Quickstart for installing and using ownCloud

## Introduction
ownCloud is an open-source file-sharing server and collaboration platform that can store your content in a centralized location. It allows you to take control of your content and security by not relying on third-party content hosting services.

This document covers the information about the following topics:

*	[How to download and install ownCloud](#installing-owncloud)
*	How to configure ownCloud
*	How to add a user account to the ownCloud server
*	How to connect to the Owncloud server using a desktop or mobile client

The following descriptions focus on the Ubuntu 18.04 server and are intended for use by professional administrator only.

## Installing ownCloud

Before setting up ownCloud, you must install Apache, PHP, and MySQL to install and configure an ownCloud server.

### Installing Apache HTTP Server

The Apache web server is a popular open source web server that can be used along with PHP to host dynamic websites.

Perform the following steps to install Apache:

1. Make sure your `apt` cache is updated with:
```
$ sudo apt update
```  
If you are using sudo for the first time, then provide your regular user’s password to validate your permissions.

2. Install Apache using the following command:
```
$ sudo apt install apache2
```
`apt` provides the information about the packages that plans to install and extra disk space to consume. 

3. Press `Y` and press ENTER to confirm.

#### Adjusting the firewall to allow web traffic  Hello

Assuming that you have followed the initial server setup instructions and enabled the UFW firewall, make sure that your firewall allows HTTP and HTTPS traffic.

To adjust the firewall, follow these steps:

1.	Check that UFW has an application profile for Apache using the following command:
```
$ sudo ufw app list
```

```
Output
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
```

2.	If you see the Apache Full profile details, you’ll see that it enables traffic to ports 80 and 443:
```
$ sudo ufw app info "Apache Full"
```

```
Output
Profile: Apache Full
Title: Web Server (HTTP,HTTPS)
Description: Apache v2 is the next generation of the omnipresent Apache web
server.

Ports:
  80,443/tcp
```

To allow incoming HTTP and HTTPS traffic for this server, run the following command:
```
sudo ufw allow "Apache Full"
```

3.	Visit your server’s public IP address in your web browser to verify by running the following command:
```
http://your_server_ip
```
 
After successful installation and accessible through your firewall, you can see the default Ubuntu 18.04 Apache web page as follows:
 
### Installing MySQL

MySQL is a database management system. It organizes and provides access to databases where your site can store information.

To install MySQL, follow these steps:

1.	Use apt to acquire and install this software:
```
sudo apt install mysql-server
```
This command shows you a list of the packages that gets installed, along with the required disk space. Enter Y to continue.

2.	When the installation is complete, run a security script that comes pre-installed with MySQL, which removes some dangerous defaults and lock down access to your database system. Start the interactive script by running:
```
$ sudo mysql_secure_installation
``` 
3.	Answer Y for yes if you want to configure the VALIDATE PASSWORD PLUGIN, or anything else to continue without enabling.
4.	Enter Y to continue for a series of questions asked.
5.	Verify if you are able to log in to the MySQL console by running the following command:
```
$ sudo mysql
```
 
6.	It connects to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You must see the output as:
```
 Output
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.34-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

7.	To exit the MySQL console, run the following command:
```
mysql> exit
```
 
### Installing PHP

PHP is the component of your setup that processes code to display dynamic content. It can run scripts, connect to your MySQL databases to get information, and provide the processed content over to your web server so that it can display the results to your visitors.

In addition to the php package, you need libapache2-mod-php to integrate PHP into Apache, and the php-mysql package to allow PHP to connect to MySQL databases. Run the following command to install all three packages and their dependencies:

```
$sudo apt install php libapache2-mod-php php-mysql
```
 
 
### Downloading and installing ownCloud

To install ownCloud binaries, you have to download the required package. After doing so, you can perform the following steps manually or use the provided scripts. These scripts are convenient since they can also be used for upgrading, which eases the process a lot.

#### Downloading ownCloud

Before downloading ownCloud, change to a directory where you want to save the file temporarily. This can be, for example, /tmp. In further examples, tar archives or the complete ownCloud bundle is used. The name for the complete archive looks like this:

```
owncloud-complete-yyyymmdd.archive_type
```
 
Download the archive of the latest ownCloud version:

1.	Go to the ownCloud Download Page and select the required package. You can download either the ```.tar.bz2``` or ```.zip``` archive. Based on the example below, copy the link of the selected file and run the following command to download it:
```
wget https://download.owncloud.org/community/owncloud-complete-yyyymmdd.tar.bz2
```
 
2.	Download the corresponding checksum file:
```
wget https://download.owncloud.org/community/owncloud-complete-yyyymmdd.tar.bz2.md5
  or
wget https://download.owncloud.org/community/owncloud-complete-yyyymmdd.tar.bz2.sha256
```
 
3.	Verify the MD5 or SHA256 sum:
```
sudo md5sum -c owncloud-complete-yyyymmdd.tar.bz2.md5 < owncloud-complete-yyyymmdd.tar.bz2
 or
sudo sha256sum -c owncloud-complete-yyyymmdd.tar.bz2.sha256 < owncloud-complete-yyyymmdd.tar.bz2
```
 
4.	You can also verify the PGP signature:
```
wget https://download.owncloud.org/community/owncloud-complete-yyyymmdd.tar.bz2.asc

gpg --verify owncloud-complete-yyyymmdd.tar.bz2.asc owncloud-complete-yyyymmdd.tar.bz2
```
 
#### Installing ownCloud

The ownCloud server package does not exist within the default repositories for Ubuntu. However, ownCloud maintains a dedicated repository for the distribution that you can add to your server.

1.	Download their release key and import it with the apt-key utility with the following command:
```
sudo wget -nv https://download.owncloud.org/download/repositories/production/Ubuntu_18.04/Release.key -O Release.key
sudo apt-key add - < Release.key
```

2.	Run the following shell commands to add the repository:
```
sudo echo 'deb http://download.owncloud.org/download/repositories/production/Ubuntu_18.04/ /' |  sudo tee /etc/apt/sources.list.d/owncloud.list
```

3.	Update repositories using the following command:
```
sudo apt-get update
```

4.	Install additional PHP packages using the following command:
```
sudo apt install php-bz2 php-curl php-gd php-imagick php-intl php-mbstring php-xml php-zip
```

5.	Install ownCloud package using the following command:
```
sudo apt-get install owncloud-files
```
 
Install Owncloud Package
 
### Configuring Apache with SSL

This section covers the information about creating a virtual host for ownCloud.

Follow these steps to configure Apache with SSL:

1.	Create a folder for SSL certificates.
```
sudo mkdir /etc/apache2/ssl
```

2.	Enable SSL module.
```
sudo a2enmod ssl
```

3.	Restart Apache.
```
sudo systemctl restart apache2
```
4.	Copy your SSL certificates to /etc/apache2/ssl/ folder.	
5.	Create a virtual host file.
```
sudo vim /etc/apache2/sites-available/fosslinuxowncloud.com.conf
```
6. Add the following lines to file. Replace ServerName with fosslinuxowncloud with anything you want.
```
<VirtualHost *:80>
    ServerName fosslinuxowncloud.com
    ServerAlias www.fosslinuxowncloud.com
    ServerAdmin webmaster@fosslinuxowncloud.com
    DocumentRoot /var/www/owncloud

<Directory /var/www/owncloud>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/fosslinuxowncloud.com-error.log
    CustomLog ${APACHE_LOG_DIR}/fosslinuxowncloud.com-access.log combined
</VirtualHost>

<VirtualHost *:443>
    ServerName fosslinuxowncloud.com
    ServerAlias www.fosslinuxowncloud.com
    ServerAdmin webmaster@fosslinuxowncloud.com
    DocumentRoot /var/www/owncloud

<Directory /var/www/owncloud>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/fosslinuxowncloud.com-ssl-error.log
    CustomLog ${APACHE_LOG_DIR}/fosslinuxowncloud.com-ssl-access.log combined


SSLEngine on

SSLCertificateFile /etc/apache2/ssl/fosslinuxowncloud.cer
SSLCertificateKeyFile /etc/apache2/ssl/fosslinuxowncloud.key
SSLCertificateChainFile /etc/apache2/ssl/fosslinuxowncloud.ca


</VirtualHost>
```
 
7.	You can give any name to your SSL certificate files.
```
SSLCertificateFile /etc/apache2/ssl/certificatefile-name.cer
SSLCertificateKeyFile /etc/apache2/ssl/certificate-key-name.key
SSLCertificateChainFile /etc/apache2/ssl/chain-certificate-name.ca
```
8.	Check the syntax of the configuration file.
```
sudo apachectl -t
```
 
9.	If you get a Syntax OK message, use this command line to disable the default, virtual host.
```
sudo a2dissite 000-default.conf
```
 
10.	The following commands should enable new virtual hosts.
```
sudo a2ensite fosslinuxowncloud.com.conf
```
 
11.	Restart Apache to activate changes.
```
sudo systemctl restart apache2
```
 
### Configuring the MySQL Database for ownCloud

This section covers the information about Configuring the MySQL Database for ownCloud.

Follow these steps to configure MySQL Database:

1.	Access MySQL using the root account.
```
sudo mysql -u root -p
```
 
2.	Create a database, and name it.
```
create database fosslinuxowncloud;
```
 
3.	Create a DB user and grant privileges.
```
create user 'ownclouduser'@'localhost' identified BY 'QB35JaFV6A9=BJRiT90'; grant all privileges on fosslinuxowncloud.* to ownclouduser@localhost;
```
 
## Configuring ownCloud
Open a web browser and navigate to the following address to access the ownCloud web interface:
```
https://Domain-Name or IP
```
 
Owncloud Web Interface
 
Follow these steps to configure ownCloud:
1.	Create an admin account by choosing a username and a password.
2.	Fill out the details of the database name, database username, and database password. Here we installed the database in the same server, so we leave the database host as “localhost”.
Configurations
 
3.	Click Finish setup to configure.
4.	Now it redirects to login page, where you can provide username and password to access the dashboard. Login Page
 
You can use a desktop or mobile client to sync your data to your ownCloud. Download OwnCloud client from here.
 
## Adding user accounts to ownCloud server

To access ownCloud service, the user needs to have their user account which is created and managed by the admin group.

**Before you begin**

1.	Sign in to the ownCloud server using an administrator account.
The user administration page is displayed.
2.	Click on the admin username to get the users menu link. A drop-down menu appears with options to enter the following sections: Personal, Users, Apps, Admin, Help, and Log out.
 
3.	Click on the Users item from the drop-down menu to view the information about your users, whose accounts have already been created.
 
4.	Create groups based on user’s access limitations.
To add groups, go to the Groups field. Hover over the + add group field, enter a name for the group.
 
Now you can start adding users and adding them to specific groups as per defined access controls.

### Adding or Creating New User Accounts

User accounts have the following properties:

For Specific Apps
FIELD	DESCRIPTION
Login Name (Username)	The unique ID of an ownCloud user, and it cannot be changed.
Full Name	The user’s display name that appears on file shares, the ownCloud
Web interface, and emails. Admins and users may change the Full Name anytime. If the Full Name is not set it defaults to the login name.
Password	The admin sets the new user’s first password. Both the user and the admin can change the user’s password at anytime.
Groups	You may create groups, and assign group memberships to users. By default new users are not assigned to any groups.
Group Admin	Group admins are granted administrative privileges on specific groups, and can add and remove users from their groups.
Quota	The maximum disk space assigned to each user. Any user that exceeds the quota cannot upload or sync data. You have the option to include external storage in user quotas.

To create a user account:

1.	Enter the new user’s Login Name and the initial Password.
2.	Optionally, assign Groups memberships.
3.	Click Create.
 
After creating the user, you may fill in their Full Name if it is different from the login name or you can leave it for the user to complete.
 
To summarize, the steps involved in creating or adding a new user are entering a name and password and delegating the user to a group. The new user can now log in to the ownCloud server and start collaborating with other users.
 
## Connecting to ownCloud server using desktop client

The ownCloud Desktop Synchronization Client is used for synchronizing files with the desktop computer. You can download the latest version of the ownCloud Desktop Synchronization Client from the ownCloud download page, and can run on various platforms. ownCloud Desktop Synchronization Client enables the user to:

•	Create folders in the home directory and keep the contents of those folders synced with the ownCloud server
•	Synchronize all the latest files irrespective of their location.

When you have installed the ownCloud Desktop Synchronization Client on your operating system, follow these steps to connect with your ownCloud server:

1.	Run the following commands to add the repository:
 ```
 wget -nv https://download.opensuse.org/repositories/isv:ownCloud:desktop/Ubuntu_18.04/Release.key -O Release.key
 ```
 
 ```
 apt-key add - < Release.key
```
2.	Run the following commands to update the repository:
 ```
 apt-get update
```
3.	Run the following commands to add the repository:
 ```
sh -c "echo 'deb http://download.opensuse.org/repositories/isv:/ownCloud:/desktop/Ubuntu_18.04/ /' > /etc/apt/sources.list.d/isv:ownCloud:desktop.list"
 ```
 
4.	Run the following to command to update the repository:
```
apt-get update
```

5.	Run the following to command to install client.
```
apt-get install owncloud-client
```
 
6.	After successful installation, launch the ownCloud Desktop Client.
7.	In the ownCloud Connection Wizard dialog box, enter the URL of your ownCloud server. Click Next to proceed.
 
8.	Enter your ownCloud login credential on the next screen. Click Next to proceed.
 
9.	Select the local folder and configure sync settings.
 
10.	Click Connect to allow the ownCloud Desktop Synchronization Client to connect to your ownCloud server. After successful connection, you will see the following options:

* Open ownCloud in Browser
* Open Local Folder

Choose the option where you want to synchronize your files. click Finish.
 
 
## Connecting to ownCloud server using mobile client

Accessing your files on your ownCloud server via the Web interface is easy and convenient. You can use any web browser on any operating system without installing special client software. However, the ownCloud application offers several key advantages over the web interface; these include:

*	A simplified interface that fits nicely on a tablet or smartphone
*	Automatic synchronization of your files
*	Share files with other ownCloud users and groups, and create multiple public share links
*	Upload of photos and videos recorded on your Android device
*	Easily add files from your device to ownCloud
*	Two-factor authentication

You can also download the ownCloud server from App Store and follow these steps to connect to the server:

1.	Enter your cloud URL and user credential which was provided during the registration process.
 
2.	Tap Connect.
 
After successful connection, you can view your files in your mobile client.
 
## Changing your ownCloud URL and port configuration

ownCloud server is accessible under the route /owncloud (which is the default, for example, https://example.com/owncloud). However, you can change this in your web server configuration, by changing the URL from https://example.com/owncloud to https://example.com/.

### Config.php Parameters

To control server operations, ownCloud uses the config/config.php file. config/config.sample.php lists all the configurable parameters within ownCloud, along with example or default values.
To do the changes on Debian/Ubuntu Linux operating systems, you need to edit these files:

*	```/etc/apache2/sites-enabled/owncloud.conf```
*	```/var/www/owncloud/config/config.php```

### Default Parameters

When config.php file is configured by the ownCloud server, you can customize the default values of the given parameters.

1.	Following parameters are configured by the ownCloud installer, and are required for your ownCloud server to operate.
```
'instanceid' => '',
```
 
2.	A valid instanceid is auto-generated by the ownCloud installer when you install ownCloud.
```
'passwordsalt' => '',
```
 
3.	The salt is used to hash all passwords and is auto-generated by the ownCloud installer. Make sure not to lose this salt.
```
'trusted_domains' =>
  array (
    'demo.example.org',
    'otherdomain.example.org',
  ),
```
 
4.	Provides a list of trusted domains that users can log into. Do not remove this, as it performs necessary security checks.
```
'cors.allowed-domains' => [
        'https://foo.example.org',
],
```
 
5.	All users can use tools running CORS requests from the listed domains.
```
'datadirectory' => '/var/www/owncloud/data',
```
 
6.	The following defines the location of the user files data/ in the ownCloud directory.
```
'version' => '',
```
 
7.	Identifies the current version number of your ownCloud installation. This is set up and updated during installation, and so it does not require any changes.
```
'version.hide' => false,
```

8.	Optionally, show the hostname of the server in status.php.
 ```
 'dbtype' => 'mysql',
```
9.	Identifies the database used with this installation.
```
'dbhost' => '',
```
 
10.	Defines the host server name, for example localhost, hostname, hostname.example.com, or the
IP address. To specify a port use hostname:##

For example,
```
'dbhost' => 'x.x.x.x:8080',
where x.x.x.x is server’s IP address
```

```
'dbname' => 'owncloud',
```
 
11.	Defines the name of the ownCloud database, which is set during installation.
```
'dbuser' => '',
```
 
12.	Defines the user that ownCloud uses to write to the database. This must be unique across ownCloud instances using the same SQL database.
```
'dbpassword' => '',
```

13.	Defines the password for the database user, which is set up during installation.
```
'dbtableprefix' => '',
```
 
14.	Prefix for the ownCloud tables in the database.
```
'installed' => false,
```
 
15.	Indicates whether the ownCloud instance was installed successfully; true indicates a successful installation, and false indicates an unsuccessful installation.

### Default config.php example

The following displays the config.php after installing ownCloud using MySQL database.
```
<?php
$CONFIG = array (
  'instanceid' => 'oc8c0fd71e03',
  'passwordsalt' => '515a13302a6b3950a9d0fdb970191a',
  'trusted_domains' =>
  array (
    0 => 'localhost',
    1 => 'studio',
    2 => '192.168.10.155'
  ),
  'datadirectory' => '/var/www/owncloud/data',
  'dbtype' => 'mysql',
   'version' => '7.0.2.1',
  'dbname' => 'owncloud',
  'dbhost' => 'localhost',
  'dbtableprefix' => 'oc_',
  'dbuser' => 'oc_carla',
  'dbpassword' => '67336bcdf7630dd80b2b81a413d07',
  'installed' => true,
);
```
 
When the changes are made and all the files have been saved, restart the Apache server. You can now access ownCloud from either, https://example.com/ or https://localhost/.

### Proxy Configurations

1.	The automatic hostname, protocol or webroot detection of ownCloud can fail in certain reverse proxy situations. This configuration allows the automatic detection to be manually overridden. You can find the detailed explanation in the Overwrite Parameters section.
 ```
 'overwritehost' => '',
 ```
2.	This option allows you to manually override the automatic detection; for example www.example.com, or specify the port www.example.com:8080.
 ```
 'overwriteprotocol' => '',
```
3.	When generating URLs, ownCloud attempts to detect whether the server is accessed via https or http. However, if ownCloud is behind a proxy and the proxy handles the https calls, ownCloud would not know that ssl is in use, which would result in incorrect URLs being generated.
 ```
 'overwritewebroot' => '',
```

4.	ownCloud attempts to detect the webroot for generating URLs automatically. For example, if www.example.com/owncloud is the URL pointing to the ownCloud instance, the webroot is /owncloud. When proxies are in use, it may be difficult for ownCloud to detect this parameter, resulting in invalid URLs.
 ```
 'overwritecondaddr' => '',
```

5.	This option allows you to define a manual override condition as a regular expression for the remote IP address. For example, defining a range of IP addresses starting with 10.0.0. and ending with 1 to 3:
```^10.0.0.[1-3]$```
 ```
 'overwrite.cli.url' => '',
```

Use these configuration parameter to specify the base URL for any URLs which are generated within ownCloud using any kind of command line tools (cron or occ). The value should contain the full base URL:
https://www.example.com/owncloud














## Welcome to GitHub Pages hello

You can use the [editor on GitHub](https://github.com/rorzen/owncloud-install-and-configure/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/rorzen/owncloud-install-and-configure/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
