# How to Install MariaDB and Database Administration Tools in Ubuntu Linux

Software versions used when writing this guide, listed here
for reference in case future versions differ from the instructions:
- Ubuntu 18.10
- MariaDB 10.1.29
- phpMyAdmin 4.6.6
- Adminer 4.6.3

**NOTE:** phpMyAdmin and Adminer perform the same basic function,
so you do not need to install both of them.
Each allows you to view and administer your databases (schema)
through a GUI rather than through the `mysql` command line interface
that comes with MariaDB.

## MariaDB

MariaDB is an open-source fork of the MySQL database server.
Due to software licensing concerns, Ubuntu provides an old version
of MySQL and a fairly recent version of MariaDB,
so we will use MariaDB to take advantage of newer features.
MariaDB maintains drop-in replacement compatibility with MySQL,
which is why many of its files have "mysql" in their name.

Install MariaDB:
```
sudo apt install mariadb-server
```

Create a secure configuration for MariaDB by running the following
command and accepting the recommended responses to the prompts.
This includes setting a password for the `root` database superuser,
which is different from the `root` user of your Linux system
but similar in concept.  
```
sudo mysql_secure_installation
```

Disable `mysql` client history file to avoid capturing passwords,
or remove "group" and "other" permissions from the file:  
```
# First, change to your home directory, where your history file is.
cd

# This will remove (truncate) currently saved history:
echo -n > .mysql_history

# This will help prevent other, non-root system accounts
# from reading your history:
chmod go-rwx .mysql_history

# This will disable `mysql` client command history altogether:
rm -f .mysql_history  && ln -s /dev/null .mysql_history
# (to undo, run `rm -i ~/.mysql_history`)
```

Create a fully privileged database user account that can connect
via IP on localhost. By default the `root` account can connect
only via Unix domain socket file, but phpMyAdmin and Adminer connect
via IP. Make up a username and password for this account; below are
only examples.  
```
# At a Terminal shell, log in to MariaDB as the root user:
sudo mysql

# Then, at the `mysql>` client prompt:
create user 'adminuser'@'localhost' identified by 'ReplaceThisWithANewPassword';
grant all on *.* to 'adminuser'@'localhost' with grant option;
```

## phpMyAdmin

Installing phpMyAdmin this way prevents a full installation of
the Apache httpd web server (we will run PHP in standalone/development mode instead):  
```
sudo apt-get --no-install-recommends install phpmyadmin
```

Create a script file `~/bin/phpmyadmin` with the following contents
(you may change the port from 8001 to another number above 1024):  
```
#!/bin/bash
cd /usr/share/phpmyadmin && php -S localhost:8001
```

Make the script file executable:  
```
chmod u+x ~/bin/phpmyadmin
```

Run the script file to start phpMyAdmin:  
```
phpmyadmin
```

Open a web browser and go to the URL where your phpMyAdmin app is
running (this will be the URL on the "Listening on …" output line
when you started phpMyAdmin):  
http://localhost:8001/

Use the database username and password you specified on the
`create user …` line in the earlier section.

To stop phpMyAdmin running when you are done with it,
go to the Terminal shell where you started it and press Ctrl-c.

## Adminer

```
sudo apt install adminer
```

Create a script file `~/bin/adminer` with the following contents
(you may change the port from 8002 to another number above 1024):  
```
#!/bin/bash
PORT=8002
echo "Browse to this URL:"
echo "  http://localhost:$PORT/adminer"
cd /usr/share/adminer && php -S localhost:$PORT
```

Make the script file executable:  
```
chmod u+x ~/bin/adminer
```

Run the script file to start Adminer:  
```
adminer
```

Open a web browser and go to the URL where your Adminer app is running.
This will be the URL on the "Listening on …" output line when you
started Adminer, followed by `/adminer`:  
http://localhost:8002/adminer

Use the database username and password you specified earlier on the
`create user …` line during MariaDB setup.

To stop Adminer running when you are done with it,
go to the Terminal shell where you started it and press Ctrl-c.
