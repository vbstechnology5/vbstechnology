---
description: these are steps to help you install moodle manually
---

# Moodle Installation Guide

### PS:For everything you need to start/stop/enable/desable/restart/reset use:

```
// sudo systemctl restart apache2 or mysql...
```

## 1- If not already installed, install (Apache, MySql, PHP) Adding the php7 ppa: sudo add-apt-repository ppa:ondrej/php

<pre class="language-sh"><code class="lang-sh"><strong>sudo apt install apache2 mariadb-client mariadb-server php7.4 libapache2-mod-php
</strong>sudo apt install apache2 mysql-client mysql-server php7.4 libapache2-mod-php
</code></pre>

`sudo apt install apache2 mariadb-client mariadb-server php7.4 libapache2-mod-php`

`·sudo apt install apache2 mysql-client mysql-server php7.4 libapache2-mod-php`

### - if you installed mariadb, set mysql root password

#### `sudo mysql_secure_installation`&#x20;

## 2- If not already installed, Install additional software

#### Note: change the versions and see if it's compatible if necessary

`· sudo apt install graphviz aspell ghostscript clamav php7.4-pspell php7.4-curl php7.4-gd php7.4-intl php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-ldap php7.4-zip php7.4-soap php7.4-mbstring`

`· sudo service apache2 restart`

`· sudo apt install git`

&#x20;`sudo apt install PHP8.2 php8.2-pspell php8.2-curl php8.2-gd php8.2-intl php8.2-mysql php8.2-xml php8.2-xmlrpc php8.2-ldap php8.2-zip php8.2-soap php8.2-mbstring php8.1-pspell php8.1-curl php8.1-gd php8.1-intl php8.1-mysql php8.1-xml php8.1-xmlrpc php8.1-ldap php8.1-zip php8.1-soap php8.1-mbstring`&#x20;

## **3-Download Moodle**

· `cd /opt`

`· sudo git clone git://git.moodle.org/moodle.git`

· OR download it from \[https://download.moodle.org/stable400/moodle-latest-400.tgz]\(https://download.moodle.org/stable400/moodle-latest-400.tgz)

`· cd moodle`

`· sudo git branch -a`

· Chose branch \_ `sudo git branch --track MOODLE_400_STABLE origin/MOODLE_400_STABLE`

· `sudo git checkout MOODLE_400_STABLE`

`· Copy moodle to _ sudo cp -R /opt/moodle /var/www/html/`

`· Create moodledata directory __ sudo mkdir /var/moodledata`

`· sudo chown -R www-data:www-data /var/moodledata`

`· sudo chmod -R 777 /var/moodledata`

`· sudo chmod -R 0755 /var/www/html/moodle`

## 4- create database for moodle and sava the credintals

· `CREATE DATABASE databaseName DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`

· `create user 'moodledude'@'localhost' IDENTIFIED BY 'passwordformoodledude';`

`· GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO 'moodledude'@'localhost';`

`· quit;`

· make sure you can't access root user with any password

## 5-Configure Moodle

&#x20;`cp config-dist.php config.php`

`sudo chown -R www-data:www-data config.php`

`· Change (dbtype, dbname, dbuser, dbpass, wwwroot, dataroot)`

## 6- Configure Apache

· after creating DNS record that point to the server

· `cd /etc/apache2/sites-available/`

`· cp 000-default.conf sitename.com.conf`

```sh
sudo nano /var/www/html/moodle
sudo nano /var/log/apache2/error.log.1 (to access error logs)
sudo nano /var/log/apache2/access.log (to access logs)
```

####

· In config file update (ServerName, documentRoot) If not sub-domain (ServerAlias)

· If there is redirect to https remove it

`· a2ensite sitename.com.conf`

· Install certbot certification

· If certbot not installed [https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04)

· `certbot --apache`

## 7- Configure Moodle from browser

· create admin account and save it

