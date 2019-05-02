---
layout: post
title: "Setting up a Virtual Private Server (CentOS 7)"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

## 1) SSH into VPS

`# ssh 1.2.3.4 -p 22 -i /path/to/id_rsa -l root`

## 2) Set your UNIX root password
Upon first login, you will be asked to set your UNIX password. Enter in the password provided to you by your service provider, and enter in your new password.

## 3) Update `yum` and install global software

`# yum update`

In General:
`# yum install gcc gcc-c++ vim epel-release unzip`

Version Controls:
`# yum install git`
`# yum install mercurial`
`# yum install subversion`

For Nginx:
`# yum install epel-release`
`# yum install nginx`

For Node Application Deployment:
`# yum install nodejs npm`
`# npm install bower pm2`

For MySQL:
`# cd /opt`

`# mkdir external-repos`

`# cd external-repos`

`# wget https://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm`

`# rpm -ivh mysql57-community-release-el7-8.noarch.rpm`

`# yum install mysql-community-server mysql-community-client mysql-community-devel mysql-utilities`

`# vim /etc/my.cnf`

Add a line below `[mysqld]` with the content `skip-grant-tables`.

`# service mysqld start`

`# mysql -uroot -p`

Hit enter when challenged for the authentication and enter the following line of code to set your root password.

`# UPDATE mysql.user SET authentication_string = PASSWORD('%_YOUR_PASSWORD_%') WHERE User = 'root' AND Host = 'localhost'; FLUSH PRIVILEGES; quit;`

Remove the `skip-grant-tables` option you added earlier to `/etc/my.cnf` and restart `mysqld`:

`# service mysqld restart`

You're done.


## 4) Change `sshd` port number
While this does not stop hackers, it does throw script kiddies off because "*oh, I can't find port 22*".

`# cd /etc/ssh`

`# vim sshd_config`

Type in `/Port 22` and hit enter, you should be brought to line 17 which looks like: 
`#Port 22`

Remove the `#` and change the `22` to your port of choice.

## 5) Create user accounts
For each user:

### 5a) Create the user
`# adduser user1`

### 5b)  Set a default password
`# passwd user1`

### 5c) Add public key into their ~/.ssh/authorized_keys file
```
# cd /home/user1
# mkdir .ssh
# cd .ssh
# vim authorized_keys
```

Copy the public key provided to you for `'user1'@'localhost'` and paste it inside this file. Ensure no trailing line feed.

Do a `chown` and `chmod` to make the `.ssh` directory and `authroized_keys` file belong to user1:

`# cd /home/user1`

`# chown user1:user1 .ssh`

`# cd .ssh`

`# chown user1:user1 authorized_keys`

`# chmod 640 authorized_keys`

`# chmod 750 .ssh`

### 5d) Grant `sudo` access
Enter in the following command to edit `sudo` access for users:
`# visudo`

Type in `/root^IALL`, where `^I` is entered by hitting the `tab` button, and hit enter. You should be brought to the last line of the following code block:
<small><small>
```
## Next comes the main part: which users can run what software on
## which machines (the sudoers file can be shared between multiple
## systems).
## Syntax:
##
##      user    MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
```
</small></small>

After the line with `root    ALL=(ALL)      ALL`, add the following line:

`user1    ALL=(ALL)      ALL`

Repeat the above for each user you wish to give `sudo` access to.

### 6) Setup groups

Create groups with the following command:
`# groupadd %_GROUP_NAME_%`

Add existing users to the group via the following command:
`# useradd -a -G %_GROUP_NAME_% %_USER_NAME_%`


### 7) Prevent `root` login
After setting up `sudo` access and SSH key authentication for your users, you should disable `root` access. 

`# vim /etc/ssh/sshd_config`

Find for the line containing `PermitRootLoin` by typing in `/PermitRootLogin`. It should be around line 49. Change the default `yes` to `no` and remove the `#` to uncomment it.

### 8) Set up Firewalld
CentOS 7 comes with `firewall` included, which if you've used `iptables` before, will be thankful for. For this next part, start up the `firewalld` service with:

`# service firewalld start`

Running the following lines to allow ports 80, 443 and `%_SSH_PORT_%` where `%_SSH_PORT_%` is the port number from Step 4 of this article (the port you will use to SSH into your VPS).

`# firewall-cmd --permanent --zone=public --add-service=http`

`# firewall-cmd --permanent --zone=public --add-service=https`

`# firewall-cmd --permanent --zone=public --add-port=%_SSH_PORT_%/tcp`

Reload the firewall with:

`# firewall-cmd --reload`

You're done here.
