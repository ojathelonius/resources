## GLPI
GLPI is a free IT Asset Management, issue tracking system and service desk system, written in PHP.

* [Download & installation](#download--installation)
* [Setting up the database](#setting-up-the-database)
* [Apache configuration](#apache-configuration)
* [GLPI installation](#glpi-installation)

### Download & installation
The information below is for CentOS only.

#### PHP
```
yum install epel-release
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
yum install php56w php56w-common php56w-mbstring php56w-mysqli php56w-gd php56w-dom (used for PHPCas)
```

#### MariaDB
yum install mariadb-server mariadb-devel
systemctl enable mariadb
systemctl start mariadb
mysql_secure_installation

### Setting up the database

```
mysql -u root -p
create database glpidb;
create user 'glpiuser'@'localhost' identified by 'admin';
grant all privileges on glpidb.* to glpiuser@localhost;
flush privileges;

```

### Apache configuration
Add an alias in the Apache configuration.
```
Alias /glpi /var/www/glpi
```

Give Apache access rights to the GLPIfolder.
```
chmod -R 755 /var/www/glpi
chown -R apache:apache /var/www/glpi
```

Make sure the glpi/files folder contains an `.htaccess` with **DENY FROM ALL**, then verify that the `<Directory "/var/www">` sets `AllowOverride` to **all**.

### GLPI installation
Go to `your-server-url/glpi/` and follow the install procedure.
