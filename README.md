## Introduction
Requirementd for this project wree to take a baseline installation of a Linux distribution on a virtual machine and prepare it to host the `catalog` web applications, include installing updates, securing it from a number of attack vectors and installing/configuring web and database servers.

---

## Table of Contents

1. [Introduction ](#introduction)
1. [Important Details ](#important-details)
    - [IP Address](#ip-address)
    - [SSH Port Number](#ssh-port-number)
    - [Terminal Login URL](terminal-login-url)
    - [Web Application URLs](#web-application-urls)
    - [Administrator Login](#administrator-login)
    - [Web Application Repository](#web-application-repository)

1. [Setup And Configurations](#setupandconfigurations)
    - [General Configuration](#general-configuration)
        *   [Get Latest Updates](#get-latest-updates)
        *   [Ugrade Packages](#upgrade-packages)
        *   [Cleanup installations and uninstalls](#cleanup-installations-and-uninstalls)
        *   [Install finger ](#install-finger)
        *   [Install nano](#install-nano)
        *   [Create New User ](#creeate-new-user)
        *   [Set SUDO Permissions](#set-sudo-permissions)
        *   [Creating A Public Key Pair](#creating-apublic-key-pair)
        *   [Disable Root Login](#disable-root-login)
        *   [Setup UFW Firewall ](#setup-ufw-firewall)
        *   [Setup Host Name](#setup-host-name)
        *   [Setup EMail](#setup-email)
        *   [Install Logwatcher](#install-logwatcher)
        *   [Install Apache](#install-apache)
        *   [Set Timezone UTC](#setup-timezone-utc)
        *   [Install Fail2Ban](#install-fail2ban)
        *   [Install PSAD](#install-psad)
        *   [Setup Automatinc Updates](#setup-automatic-updates)
        *   [Install AptiCron](#install-apticron)
        *   [Setup PostgresSql](#setup-postgressql)

    - [Web Application Configuration and Setup](#web-appliation-configuration-and-setup)
    - [Creating The Database ](#creating-the-database)   
    - [Populate Basic Data](#populate-basic-data)   
    - [Running The Application ](#running-the-application)
1. [Extra Credit Features](#extra-credit-features)
1. [Monitoring Scripts ](#monitoring-scripts)
1. [References](#references)

---

#Important Details

###IP Address 

The public IP address of the samscatalogapp server is  `54.149.100.110`

###SSH Port Number

The samscatalogapp server can be connectes via SSH on port  `2200`

###Terminal Login URL

You can use the following url to loging through a terminal.

```python
 
 ssh -i ~/.ssh/udacity_key.rsa root@54.149.100.110 -p 2200

```

 or

```python
 ssh -i ~/.ssh/grader_key.rsa grader@54.149.100.110 -p 2200

```

 - **Please note the root user remote login has been disabled for security so you will have to use grader account.**
 - **Root users key will be provided in the student notes.**
 - **Grader urers key will be provided in the student notes.**


###Web Application URLs

You can access the web application in any one of the following ways

- **Access the site using the domain name**   [Sams Catalog App][http://www.samscatalogapp.com]
- **Access the site using the public ip**   [54.149.100.110][http://54.149.100.110]
- **Access the amazonws public domain**   [ec2-54-149-100-110.us-west-2.compute.amazonaws.com][http://ec2-54-149-100-110.us-west-2.compute.amazonaws.com]

###Administrator Login

The administratior login for the web application is as follows

| User Name |Password|
|:---------:|:---------------------:|
|   admin@itemcatalog.com  |         123456        | 


###Web Application Repository

The code for the samscatalog app can be downloaded from the `feature/prod-changes` branch from the [catalog github repository][coderepo]


**[Back to top](#table-of-contents)**
---    


#Setup And Configurations
##General Configuration

Following are the configuration changes and details of the new softwares that were installed to make the web application work.

### Get Latest Updates

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to update the package information.

```python
sudo apt-get update
```

### Ugrade Packages

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to upgrade desired packages.

```python
sudo apt-get upgrade
```

### Cleanup installations and uninstalls

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to remove unrelated packages. T

```python
sudo apt-get autoremove
```


### Install finger 

**finger** is a user information lookup program.

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to install finger. Refer to the [finger man page][finger]  for details on finger.

```python
sudo apt-get install finger
```

You can now use finger to get user information like below

```python
finger grader
```

### Install nano

**nano** is a small and friendly text editor

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to install finger. Refer to the [nano man page][nanoman]  for details on nano.

```python

sudo apt-get install nano
```

You can now use nano to edit files

```python

nano /etc/sudoers
```

### Create New User 

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in with `root` user login  use the following command to create a new user **grader**.

```python
adduser grader
```

if you are logged in as any other user with sudo permissions use the following command

```python
sudo adduser grader
```

follow the on-screen prompts to create the new user grader and provide additional information about the new user.

### Set SUDO Permissions

In case you want to allow the user to use sudo performa the following steps

- Login to the server using root or any other account with sudo permissions
- Use the following command to see the contents of the **sudoers** file
        `cat /etc/suoders `
- Make sure that the sudo permissions are uncommented and set same as root. They should look like..

```python
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```
- if modifications are required use the following command to edit the sudoers file and make required changes.

```python
    sudo nano /etc/sudoers

```
- Next use the navigate to the **sudoers.d** directory and create a new file  called **grader**

```python
# navigate to the directory 
sudo cd /etc/sudoers.d

#Create a new file called grader
sudo touch grader

#open the file in nano
sudo nano grader
```
- Add the following lines to the file save and close the file 

```python
# adding permissions for the grader user
grader ALL=(ALL) NOPASSWD:ALL
```

- finally change the file permissions on the file just created.

```python
sudo chmod 644 grader
```

### Creating A Public Key Pair

- On your computer open a terminal program and use  **ssh-keygen**  to create a public key pair. Use the following command and follow the on screen instructions

```python
ssh-keygen -C 'grader@54.149.100.110' 

```
- When prompted for a filename create something like **grader_key.rsa**
- Navigate to the directrory where you saved the key and open the file in a text editor
- Copy the contents of the file to clipboard

- Login to the server and use the following commands to update the keys.

```python

# login to the server using with grader user
ssh -i ~/.ssh/udacity_key.rsa grader@54.149.100.110

# create a directory under /home/grader/
mkdir .ssh

#Create a  new file  for authorized_keys  to store the public key
touch .ssh/authorized_keys

#Edit and save the key copied from the computer
nano .ssh/authorized_keys

#Change the permissions on the **.ssh** directroy
chmod 700 .ssh

#Change the permission on the key file
chmod 644 .ssh/authorized_keys

#logoff from the server

```

- Back on your computer in the terminal use the following command for logging in to the server

```python
ssh -i ~/.ssh/udacity_key.rsa grader@54.149.100.110

```

### Disable Root Login

- This will prevent the majority of brute force attack attempts.
- Be very careful when making the following changes. You can lock youself out.

- Use nano to edit `/etc/ssh/sshd_config` and make the following changes

```python
sudo nano /etc/ssh/sshd_config
```

- Switch to another ssh port number rather than the default ssh port 22. This will prevent the majority of brute force attack attempts.

```python
#change the port to 2200

#Port 22
Port 2200

```

- Disable ‘root’ login. Allow only members of the ‘sudo’ group to login. 

```python
#Stop root from logging in remotely
PermitRootLogin no

#Allow only sudo group members to login remotely
AllowGroups sudo
```

- Restart the ssh service 

```python

restart ssh

```

- **This point onwards when logging in to the server from any terminal you will have tospecify the port number like so. Logoff and test the login.

```python

ssh -i ~/.ssh/grader_key.rsa grader@54.149.100.110 -p 2200

```
### Setup UFW Firewall 

Setup the default firewall configuration tool for Ubuntu **UFW**. For details refer to the [UFW][ufwhelp] page.

```python

sudo nano /etc/wfw/applications.d/openssh-server

#change the ports to 2200
ports=2200/tcp

```
- Set up firewall rules use the following commands. 
- Disable all incomming by default and allow the ports that you want to use

```python

#Deny all incoming
sudo ufw default deny incoming

#Allow incoming ssh connections
sudo ufw allow ssh

#Allow tcp connection on the new ssh port
sudo ufw allow 2200/tcp

# allow www on default port 80
sudo ufw allow www

# allow the ntp 
sudo ufw allow ntp

#disable the fire wall
sudo ufw disable

#enable the filrewall
sudo ufw enable

# enable firewall logs 
sudo ufw logging low

#verify the status of ufw
sudo ufw status verbose

# you should see output like 

Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
2200/tcp                   ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
123                        ALLOW IN    Anywhere
22                         ALLOW IN    Anywhere
2200/tcp (v6)              ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
123 (v6)                   ALLOW IN    Anywhere (v6)
22 (v6)                    ALLOW IN    Anywhere (v6)


```
### Setup Host Name

- This is **optional* in case you have registered domain.
- Use nano to edit the **/etc/hosts** and **/etc/hostnames** files
```python
sudo nano /etc/hosts

#Edit the following line and add the domain name
# in this case samscatalogapp.com

127.0.0.1  samscatalogapp.com localhost

#save and exit

#edit the hostnames file
sudo nano /etc/hostnames

#Edit the file and add the domain name
# in this case samscatalogapp.com
samscatalogapp.com

#save and exit

#register the newaliases
sudo newaliases

#reboot the server for the changes to take affect
reboot
```


### Setup EMail

Setup a email client for sending mails to local and external users.

- Install **sendmail**

```python

#get the updates
sudo apt-get update

# install the sendmail 
sudo apt-get install sendmail

```

- Update the host information **/etc/local-host-names** file
- Use the following commands to configure send mail

```python
#Edit the file 
sudo nano /etc/local-host-names

#Edit the file and add the domain name
# in this case samscatalogapp.com
samscatalogapp.com

#save and exit

# restart the networking service
sudo /etc/init.d/networking restart  

# restart the host name servie
sudo service hostname restart 

# take a backup of the file /etc/mail/sendmail.mc 
cp /etc/mail/sendmail.mc /etc/mail/sendmail.mc.original

#edit the sendmail.mc file to add host
sudo nano /etc/mail/sendmail.mc 

#edit the file and add the domain configuration
define('confDOMAIN_NAME', 'samscatalogapp.com')dnl

#complete the configuration 
sudo make -C /etc/mail/

```

### Install Logwatcher

Install and configure logwatcher to run daily to ensure you get a daily summary mail of system activity. 
- Use the following commands to install and configure logwatcher

```python

#get the updates
sudo apt-get update

# install the logwatch 
sudo apt-get install logwatch

#Make a tmp directory for logwatcher
sudo mkdir /var/cache/logwatch

#Take a backup of the default configurations file
sudo cp /etc/logwatch/conf/logwatch.conf /etc/logwatch/conf/logwatch.conf.orig


#Edit the file and change the desired settings
sudo nano /etc/logwatch/conf/logwatch.conf

#Change who recieves the mails from logwatcher
MailTo = v2saumb@gmail.com root grader

#Change the from email address
MailFrom = sysadmin@samscatalogapp.com

#Change the Reange of data to check to ALL or whatever you choose
Range = ALL

#Change the level of details that you want to recieve
Detail = Med

#Set the servvice to what you need
Service = All

#save and exit

```
- Set up a cron to run daily. The details of the logwatcher script are covered in the Scripts secion. I have set it up to run twice daily.

```python
#Add the logwatcher script to  cron
#edit and add the following lines
sudo crontab -e

# logwatcher script runs twice a day at 11:45 AM and 23:45 PM
45  11,23 * * * /etc/monitoringscripts/logwatcher


```
### Install Apache
- Install apache2 the web server where our web appliation will run.
- We will cover the configureation in the Application Configurations section.
- Install the required libraries and modules

```python
# get the latest updates
sudo apt-get update

#install apache2
sudo apt-get install apache2

#install python module this will be used later
sudo apt-get install python-setuptools libapache2-mod-wsgi

#install the python-dev
sudo apt-get install libapache2-mod-wsgi python-dev

#install pip for later use
sudo pip install

#restart apache
sudo service apache2 restart


```
- You can now verify that you see the default apache welcome page on port 80.


### Set Timezone UTC

- Use the following command to change the timezone. Follow the on screen options to choose UTC.

```python
sudo dpkg-reconfigure tzdata

```

### Install Fail2Ban

**Fail2ban** automatically adds firewall rules to block suspicious users. 


```python
# Get the updates
sudo apt-get update

#install fail2ban
sudo apt-get install fail2ban

# take the backup of the configuration file
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local

#edit the configuration file to change the default settings
sudo nano /etc/fail2ban/jail.local

#change the following settings in the configuration file.

# Increase the ban time,the bantime controls how many seconds an offending member is banned for

bantime = 1800

#The findtime specifies an amount of time in seconds 

findtime = 600  

#The maxretry directive indicates the number of attempts to be tolerated within the findtime

maxretry = 6     

#Set what program will be used to send the alerts

mta = sendmail

# Set who will recieve th email alerts.

destemail = youraccount@email.com


# setup a default sender name for the alert mails

sendername = Fail2BanAlerts

#Use the action_mwl action, which does the same thing, 
# but also includes the offending log lines that triggered the ban:

action = %(action_mwl)s

# Enable monitoring on for Apache

...

[apache]
enabled  = true

...

[apache-noscript]
enabled  = true

...

[apache-overflows]
 enabled  = true


# Copy [apache-overflows]  section and create additional checks for bad bots and nohome

[apache-badbots]

enabled  = true
port     = http,https
filter   = apache-badbots
logpath  = /var/log/apache*/*error.log
maxretry = 2


[apache-nohome]

enabled  = true
port     = http,https
filter   = apache-nohome
logpath  = /var/log/apache*/*error.log
maxretry = 2

# enable monitoring or PHP

[php-url-fopen]
enabled = true


#Change the port for ssh and ssh-ddos and enable monitoring on those ports

[ssh]

enabled  = true
port     = 2200


[ssh-ddos]

enabled  = true
port     = 2200


# save and exit the file

# iptables-persistent to allow the server to automatically set up our firewall rules at boot. 
#These can be acquired from Ubuntu's default repositories

sudo apt-get install iptables-persistent

#Stop the service
sudo service fail2ban stop

#Start the fail2ban service
sudo service fail2ban start

```


### Install PSAD

PSAD (Port Scan Attack Detection)  will actively monitor your firewall logs to detect if port scan type attacks are in progress. 
We can configure it to alert administrators of active threats or to automatically block such attacks. 

- Turn on logging in  UFW
- Append some LOG rules to /etc/ufw/before.rules


```python

# get the latest updates
sudo apt-get update

# install PSAD
sudo apt-get install psad


#update the rules /etc/ufw/before6.rules

sudo  nano /etc/ufw/before6.rules

#add the following rules just before the  commit

-A INPUT -j  LOG
-A FORWARD -j LOG

#save and exit



#update the rules /etc/ufw/before.rules

sudo neno /etc/ufw/before.rules

#add the following rules just before the  commit

-A INPUT -j  LOG
-A FORWARD -j LOG

#save and exit

#stop the ufw service
sudo ufw disable

#start the ufw service
sudo ufw enable

# configure psad for your environment

#navigate to the psad directory

sudo cd /etc/psad

#take the backup of the conf file. Just in case some thing goes wrong
sudo cp psad.conf psad.conf.original

#Edit the psad.conf file and change the required files
sudo nano  /etc/psad/psad.conf

#change the emal address to recuegw
EMAIL_ADDRESSES             v2saumb@gmail.com;

# change the host name for the server
HOSTNAME                    samscatalogapp.com

#verify the log file path
IPT_SYSLOG_FILE             /var/log/syslog;

# update udp ports to ignore
IGNORE_PORTS                udp/53;

# Specify the danger level cautiusly or You're going to 
# spam yourself with email if you set this too low.
EMAIL_ALERT_DANGER_LEVEL    3;

# One email only per IP threat only.
EMAIL_LIMIT                 1;

# reload PSAD
sudo psad -R


# update signatures with the latest threat signatures.
psad --sig-update

# import the signatures by restarting psad
sudo psad -H


# Verify PSAD is working.
sudo psad --Status

````
### Setup Automatinc Updates

The unattended-upgrades package can be used to automatically install updated packages, and can be configured to update all packages or just install security updates. First, install the package by entering the following in a terminal:

```python

#install the unattended upgrades package
sudo apt-get install unattended-upgrades

#reconfigure the unattended upgrades
sudo dpkg-reconfigure unattended-upgrades

# Edit the configuration file and modify the required settings
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades

...
#un comment to allow the updates
 "${distro_id}:${distro_codename}-updates";

...
#Configure the email address to get information about the updates
Unattended-Upgrade::Mail "v2saumb@gmail.com";

...
#Un comment and set to true to recieve a mail on error
Unattended-Upgrade::MailOnlyOnError "true";

#save and exit.

#Set up periodic updates edit the file and configure when you want to run the different options. The values are days
sudo nano /etc/apt/apt.conf.d/10periodic


#uncomment and change values where required

APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";


#save and exit


```


### Setup AptiCron

**Apticron** will configure a cron job to email an administrator information about any packages on the system that have updates available, as well as a summary of changes in each package.

- Install the apticron package, in a terminal enter:

```python

#install apticron
sudo apt-get install apticron

#After installation edit /etc/apticron/apticron.conf, to set the email address and other options:

sudo nano /etc/apticron/apticron.conf

... 

#change who to send mail to 
EMAIL="grader@samscatalogapp.com"

```
### Setup PostgresSql
**PostgreSQL** or  **postgres** as it is commonly known, is a popular database management system. 

- To install use the following commands

```python

# get the latest updates
sudo apt-get update


#install the postgressql and postgressql-contrib
sudo apt-get install postgresql postgresql-contrib

```

- Update he postgres configuration to stop remote login. This will protect the system from potential attacks

```python
# edit the 
sudo nano /etc/postgresql/9.3/main/pg_hba.conf

# uncomment and modify the settings where necesary

# Database administrative login by Unix domain socket
local   all             postgres                                peer

# "local" is for Unix domain socket connections only
local   all             all                                     peer

# IPv4 local connections:
host    all             all             127.0.0.1/32            md5

# IPv6 local connections:
host    all             all             ::1/128                 md5


# save and exit
```

### Install virtualenv and virtualenvwrapper
**virtualenv** helps you creates a folder that stores a private copy of python, pip, and other Python packages. 

- This also gives you the flexibility to then enable this private folder while working a project. 
- This also allows you to have different versions of Python and Python packages on a per project basis.

**vitualenvwrapper** is a wrapper around the vireualenv that gives easy to understand commands and syntax.

- Follow these commands to install virtualenv and virtualenvwrapper

```python

#install virtualenv
sudo pip install virtualenv

#install virtualenvwrapper
sudo  pip install virtualenvwrapper

```

- Setup virtualenv


```python
 # export the WORKON_HOME variable, this contains the directory where our virtual environments arestored. 

export WORKON_HOME=~/.virtualenvs

# Create the new directory
mkdir $WORKON_HOME


# put the export in our ~/.bashrc file. This way our variable will get automatically defined

echo "export WORKON_HOME=$WORKON_HOME" >> ~/.bashrc

```

- Setup virtualenvwrapper


```python
#Make the virtualenv function available in our~/.bashrc

echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc

# ensure that if pip creates an extra virtual environment, it is also placed in our WORKON_HOME directory

echo "export PIP_VIRTUALENV_BASE=$WORKON_HOME" >> ~/.bashrc 

#Source ~/.bashrc to load the changes
source ~/.bashrc

```

**[Back to top](#table-of-contents)**
--- 

##Web Application Configuration and Setup
In this section we will configure and install programs required bythe **samscatalogapp** web application.

### Setup Catalog Database

- We already installed postgres earlier. 
- catalogdb will store all the data for the samscatalogapp web application.
- follow the followign commands to create the **catalogdb**

```python

# switch user to the default postgres user
su postgres

#start the psql command line interface;
psql

# create a admin user for the catalogdb
create user catalogadmin with createdb createrole createuser ;

#alter the admin user and add password
alter user catalogadmin with password 'xxxx';

#create a new ueer with transactional previlages
create user catalog with password 'xxxx';


#verify that the users are created with requied privilages
\du

 Role name   |                   Attributes                   | Member of
--------------+------------------------------------------------+-----------
 catalog      |                                                | {}
 catalogadmin | Superuser, Create role, Create DB              | {}
 postgres     | Superuser, Create role, Create DB, Replication | {}



#create the new database for storing catalog information
create database catalogdb

#exit the psql interface 
\q

# on the terminal login to the new database using the post gres user
psql -d catalogdb -U postgres

#lock down schema permissions

#revoke/ stop public from accessing the catalogdb
revoke all on database catalogdb from public;

#revoke / stop public from accessing the public schema in catalog db
revoke all on schema public from public;


# Grant all permissions on the new catalogdb to catalog admin
grant all on database catalogdb to catalogadmin;


#Grant connect permission to the catalog user
grant connect on database catalogdb to catalog;

#set permissions for the catalog transactional user

# alter the default permissions so that catalog user has select insert and update on all new tables created  

alter default privileges in schema public grant select, insert,update on tables to catalog;

#alter defaust privileges so that catalog user has all privileges on the new sequences in the public schema
alter default privileges in schema public grant all on sequences to catalog;

#Grant permission to the catalog user to access database usage stats
grant usage on schema public to catalog;

# quit the PSQL cli
\q

# Verify  that logins are working. Login using the following syntax.

# For catalogadmin user
psql -h localhost -d catalogdb -U catalogadmin

# or for catalog user
psql -h localhost -d catalogdb -U catalog

```

### Configure Apache

Now that the database is ready and we have installed the required basic software, its time to configure apache to make our application run on it.

- Create application directory

```python
#Create  A directory where the web application will live.

sudo mkdir /var/www/samscatalogapp


#create a directory where the python application will live.

sudo /var/www/samscatalogapp/samscatalogapp


```

- Update Apache2 Configurations


```python

# take a backup of the exixting configuration file

sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.original 

# open the apache2.conf for editing
sudo nano /etc/apache2/apache2.conf

# reduce the default timeout to 100 seconds
...
Timeout 100

# in the directories section add tha mapping for the application directory
... 

<Directory /var/www/samscatalogapp/samscatalogapp/>
     Options Indexes FollowSymLinks Includes
    AllowOverride None
    Order allow,deny
    Allow from all

</Directory>

#save and exit the file

```
- Configure A Virtual Host




### Install Git
### Get Application Code
### Create A virtual Environment
### Install And Configure virtualenvwrapper
### Create A Virtual Environment
### Install Rqquired Libraries 
### 


## References
1. [Apache Configurations Video][apacheconfvid]
1. [Initial Server Setup][initservsetup]
1. [Automatic Updates][automaticupd]
1. [Udacity Forum :- Project 5 Resources][udacityforum1]
1. [Logwatch Installation Guide ][logwatch]
1. [Flask Setup ][flasksetup]

[apacheconfvid]: https://www.youtube.com/watch?v=UrPNg4tWjUI
[initservsetup]: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-12-04
[automaticupd]: https://help.ubuntu.com/lts/serverguide/automatic-updates.html
[udacityforum1]: https://discussions.udacity.com/t/project-5-resources/28343
[logwatch]: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-logwatch-log-analyzer-and-reporter-on-a-vps
[flasksetup]: http://killtheyak.com/use-postgresql-with-django-flask/
[finger]: http://manpages.ubuntu.com/manpages/hardy/man1/finger.1.html
[coderepo]: https://github.com/v2saumb/catalog/tree/feature/prod-changes
[nanoman]: http://www.nano-editor.org/dist/v2.0/nano.html
[ufwhelp]: https://help.ubuntu.com/community/UFW
[fail2banapache]:https://www.digitalocean.com/community/tutorials/how-to-protect-an-apache-server-with-fail2ban-on-ubuntu-14-04
[fail2banssh]: https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-ubuntu-14-04
https://www.ftmon.org/blog/secure-ubuntu-server/
https://help.ubuntu.com/community/Logwatch
https://discussions.udacity.com/t/p5-how-i-got-through-it/15342