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
        1. []()
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

###1. Get Latest Updates

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to update the package information.

```python
sudo apt-get update
```

###2. Ugrade Packages

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to upgrade desired packages.

```python
sudo apt-get upgrade
```

###3. Cleanup installations and uninstalls

If not already logged in log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to remove unrelated packages. T

```python
sudo apt-get autoremove
```


###4. Install finger 

**finger** is a user information lookup program.

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to install finger. Refer to the [finger man page][finger]  for details on finger.

```python
sudo apt-get install finger
```

You can now use finger to get user information like below

```python
finger grader
```

###5. Install nano

**nano** is a small and friendly text editor

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in using `grader` use the following command to install finger. Refer to the [nano man page][nanoman]  for details on nano.

```python

sudo apt-get install nano
```

You can now use nano to edit files

```python

nano /etc/sudoers
```

###6. Create New User 

Log in to the environment using the [ssh url][#terminal-login-url] if you are logged in with `root` user login  use the following command to create a new user **grader**.

```python
adduser grader
```

if you are logged in as any other user with sudo permissions use the following command

```python
sudo adduser grader
```

follow the on-screen prompts to create the new user grader and provide additional information about the new user.

###7. Set SUDO Permissions

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

###8. Creating A Public Key Pair

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

###9. Disable Root Login

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
###10. Setup UFW Firewall 

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
###12. Setup Host Name

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


###13. Setup EMail

Setup a email client for sending mails to local and external users.

- Install **sendmail**

```python

#get the updates
sudo apt-get update

# install the sendmail 
sudo apt-get install sendmail

```

- Update the host information **/etc/local-host-names** file
```python


```


**[Back to top](#table-of-contents)**
---    


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
https://www.ftmon.org/blog/secure-ubuntu-server/
https://help.ubuntu.com/community/Logwatch
https://discussions.udacity.com/t/p5-how-i-got-through-it/15342