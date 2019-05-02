---
layout: post
title: "Creation & Restoration of MySQL dumps"
published: true
---

This post addresses how to create and restore MySQL dumps using `mysqldump` and the `mysql` console. The reader should have MySQL installed on their local machine with default settings.

---

## Dump Creation
Creation of dumps is done via the `mysqldump` command. On Mac it should be located at `/usr/local/mysql/bin`. Fire up your Terminal and enter `mysqldump`. If no such command is found, you need to add the path to your `~/.bash_profile` file. On Windows, you'll need to add it to your Environment Variables but from my experience of Windows, the MySQL installer works well out of the box and you should already have it in your paths. For the Mac/Linux users, see the Other Notes section at the end of this post on how to add `mysqldump` to your $PATH variable.

Choose a database schema you want to back up. We assume it is named `dbtodump` for the sake of this example. Replace it with the name of the actual schema you wish to create a dump for. Run the following command to dump the entire database to a file:
```
# mysqldump -uroot -p 
  --databases dbtodump 
  > schema-dbtodump-`date +%Y%m%d%H%M%S`.sql
```
The `date +%Y%m%d%H%M%S` portion adds a timestamp to the end of the filename so we can run the same command multiple times without overwriting the previous version. 

If you only want the schema without the data, run the following command instead. Note the additional `--no-data` flag.
```bash
# mysqldump -uroot -p 
  --databases dbtodump 
  --no-data 
  > schema-dbtodump-`date +%Y%m%d%H%M%S`.sql
```

This will create a file called `schema-dbtodump-%DATE%.sql` where %DATE% is the current timestamp.

## Dump Restoration
Dump restoration is done by using the `mysql` client. 

Assuming the above process was completed and a file named `schema-dbtodump-20160329141623.sql` exists, we can restore the `dbtodump` database by using the following command:
```
# mysql -uroot -p 
  dbtodump < schema-dbtodump-20160329141623.sql
```

## Conclusion
Creating MySQL dumps allows you to backup your database as well as to share your database schema with others on your development team. While it works, it has the setback of not retaining data on the database being updated.

For updating of schemas with data persistence after the update, use MySQL Workbench's Database Synchronization feature which will generate SQL scripts for you to safely update a model.

Till next time!

## Other Notes

### Adding `mysqldump` To $PATH
Here's how:

```
# vim ~/.bash_profile
```
This opens the file you need to add lines to. Scroll to the line starting with `export PATHS=...`. At the end of the line will be a `... :$PATH"`. Add in the following just before the `$PATH`:
```
/usr/local/mysql/bin
```
The final ending of the file should now look like:
```
... :/usr/local/mysql/bin:$PATH"
```
Refresh the configuration by exiting vim (`:wq`) and running the following line:
```
# . ~/.bash_profile
```
