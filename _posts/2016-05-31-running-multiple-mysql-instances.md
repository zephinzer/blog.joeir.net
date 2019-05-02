---
layout: post
title: "Running Multiple MySQL Instances"
published: true
---

> NOTE: This was a post migrated from my previous Ghost blog. If links don't work and you would really like to see it, it's probably posted somewhere in this blog.

[Mooziq](https://mooziq.sg) v4 is reaching it's final stages in development before the open alpha and I am now testing if the database synchronization will play nice. Lots of tables have been modified as we added in features based on user feedback from  our current v2 as well as use cases we found ourselves needing.

I came across the need to create multiple instances of the MySQL on my local computer so that I could test the database upgrade code before performing the synchronisation with our production server.  While there are resources online on doing this, none of them worked for my system, so here's a little note describing what I had to go through to get it done.

For reference, I'm running OS X Yosmite 10.10.5 with MySQL (Community Edition) 5.6.24 installed via the MySQL Installer for Mac.

## 1. Prevent MySQL Auto-Restart
If you've tried to shut down the MySQL daemon on your Mac, chances are it will auto-restart. This is because of the following *.plist* file:

`/Library/LaunchDaemons/com.mysql.mysql.plist`

Open it and find the line with `<key>KeepAlive</key>`, underneath that line, change `<true />` to `<false />`.

## 2. Shutdown MySQL
Shut down the currently running MySQL service by going to your System Preferences > MySQL > Shutdown MySQL Server.

## 3. Setup my.cnf
Open Terminal and go to `/usr/local/mysql`. Open up `my.cnf`. If it has never been changed, you should have a sole `[mysqld]` header. In anyways, add in the following 3 sections:

```mysql-conf
[mysqld_multi]
mysqld = /usr/local/mysql/bin/mysqld_safe
mysqladmin = /usr/local/mysql/bin/mysqladmin
user = _mysql

[mysqld1]
server_id       = 1
socket          = /tmp/mysql.sock
port            = 3306
pid-file        = /usr/local/mysql/data/mysqld.pid
basedir         = /usr/local/mysql
datadir         = /usr/local/mysql/data
language        = /usr/local/mysql/share/english
user            = _mysql

[mysqld2]
server_id       = 2
socket          = /tmp/mysql.sock2
port            = 3307
pid-file        = /usr/local/mysql/data2/mysqld2.pid
basedir         = /usr/local/mysql
datadir         = /usr/local/mysql/data2
language        = /usr/local/mysql/share/english
user            = _mysql
```

Take note of the differing port/socket/datadir lvalues, we need to create those directories.

## 4. Create data directories
Taking the configuration of `[mysqld2]` from the example above, the `datadir` is `/usr/local/mysql/data2`. Navigate to `/usr/local/mysql` and run the command below to initialise a new database. This command assumes your MySQL user is `_mysql`.

`scripts/mysql_install_db --user=_mysql --datadir=./data2`

## 5. Run mysqld_multi
To run `mysqld_multi` and start both servers, enter in the following command: 

`sudo mysqld_multi --defaults-file=./my.cnf start`

If no error appears, confirm the servers are running by using the following command:

`sudo mysqld_multi --defaults-file=./my.cnf report`

- - -

The part which was most often left out in tutorials was not stating the requirement for the `--defaults-file` flag which is required for installations of MySQL from the Mac Installer for MySQL.

Till the next time!
