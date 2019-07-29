# How to Install MariaDB and Database Administration Tools in Ubuntu Linux

Software versions used when writing this guide,
referenced here in case future versions are incompatible with the instructions:
- Ubuntu Desktop 19.04
- MariaDB 10.3.13
- phpMyAdmin 4.6.6
- Adminer 4.7.1

**NOTE:** phpMyAdmin and Adminer perform the same basic function,
so you do not need to install both of them.
Each allows you to view and administer your databases (schema)
through a GUI rather than through the `mysql` command line interface (CLI)
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
# First, change to your home directory, where your history file is:
cd

# This will remove (truncate) currently saved history:
echo -n > .mysql_history

# This will help prevent other, non-root system accounts
# from reading your history:
chmod go-rwx .mysql_history

# This will disable `mysql` client command history altogether:
rm -f .mysql_history && ln -s /dev/null .mysql_history
# (to undo, run `rm -i ~/.mysql_history`)
```

Create a fully privileged database user account that can connect
via IP on localhost. By default, the `root` account can connect
only via Unix domain socket file, but phpMyAdmin and Adminer connect
via IP. The username and password below are only examples;
**make up your own password**.  
```
# At a Terminal shell, log in to MariaDB as the root user:
sudo mysql

# Then, at the `mysql>` client prompt:
create user 'adminuser'@'localhost' identified by 'ReplaceThisWithANewPassword';
grant all on *.* to 'adminuser'@'localhost' with grant option;
```

In the above example, `adminuser` is the username and
`ReplaceThisWithANewPassword` is the password that would be used
to log in to phpMyAdmin or Adminer later.

If you later forget the password you created for this database user,
run this to set a new password:  
```
# At a Terminal shell:
sudo mysql

# At the `mysql>` prompt:
alter user 'adminuser'@'localhost' identified by 'ReplaceThisWithANewPassword';
flush privileges;
```

_(This last part is optional.)_ By default, Ubuntu will keep
the MariaDB database server running silently in the background,
even after the system restarts. If you do not want MariaDB to run continuously
(e.g., for increased security or reduced memory consumption),
but instead want it to run only when you manually start it,
then use these commands:  
```
# This command disables automatic start of MariaDB
# and would need to be run only once to take effect permanently:
sudo systemctl disable mysql

# To start and stop MariaDB manually when automatic startup is disabled:
sudo service mysql start
sudo service mysql stop

# This command re-enables automatic start of MariaDB
# and would need to be run only once to take effect permanently:
sudo systemctl enable mysql
```


## Set up your home bin directory

These steps create a personal space to store executable scripts
that will let you run phpMyAdmin or Adminer with one simple command.

Create a `bin` directory in your home directory:  
```
mkdir -p ~/bin
```

Add this new `bin` directory to your `PATH`.
This will let you run files directly inside `~/bin` as commands,
regardless of which directory you are working in at the time.
Edit your `~/.bashrc` file and add the following line at the end:  
```
export PATH="${PATH}:${HOME}/bin"
```

Open a new Terminal tab for this `PATH` setting to take effect.


## phpMyAdmin

Install phpMyAdmin:  
```
sudo apt-get --no-install-recommends install phpmyadmin
```

The `--no-install-recommends` option prevents a full installation of
the Apache httpd web server. We will run PHP in standalone/development mode
on demand, instead of running it as a module of a continuously running
httpd background service.

When prompted to choose which web servers to configure automatically
to run phpMyAdmin, do not select any of the servers listed. Just press Tab
to select the Ok button and press Enter to skip configuring a web server.

When asked, "Configure database for phpmyadmin with dbconfig-common?",
choose No. We've already configured MariaDB with an admin user for phpMyAdmin.

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

Open the Firefox web browser in Ubuntu and go to the URL where
your phpMyAdmin app is running (this will be the URL on the "Listening on …"
output line when you started phpMyAdmin):  
http://localhost:8001/

Log in to phpMyAdmin with the database username and password
you specified earlier in the `create user …` command during MariaDB setup.

To stop phpMyAdmin running when you are done with it,
go to the Terminal shell where you started it and press Ctrl-c.


## Adminer

In addition to MariaDB and MySQL, Adminer can manage other data storage systems
as well, such as SQLite, PostgreSQL, Elasticsearch, and MongoDB.

Install Adminer:  
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
