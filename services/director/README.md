---
description: Describe the Director website management platform
---

# Director

**Director**, also known as **web3**, is a website management interface for student and activity websites. It was created during the 2016-17 school year, and is currently used by many of the web application development classes.

The application was created to provide a secure, beginner friendly, and highly customizable website hosting platform. To achieve this goal, many features, including a web terminal and online editor, were implemented. The project has been completely [student-built](https://director.tjhsst.edu/about), and was the senior research project of [Eric Wang](user:2017ewang/README.md).

## Technologies Used

* Python
* Django
* PostgreSQL
* nss-pgsql
* NFS

## Server Configuration

Director currently runs on [sirius](../../machines/sun-servers/sirius.md). It currently requires a minimum of 8 GB of RAM due to all of the sites that are running on it.

Director uses [openldap1](./) to retrieve user IDs and virtual machines run on [deneb](../../machines/sun-servers/deneb.md).

Director uses [postgres1](../../technologies/dbs/postgresql.md) and [mysql1](../../technologies/dbs/mysql.md) for databases.

**Director Path**

* /usr/local/www/director/

**Director Configuration Files**

* /etc/nginx/nginx.conf
  * Sites: /etc/nginx/director.d/\*
* /etc/supervisor/supervisord.conf
  * Sites: /etc/supervisor/director.d/\*
* /etc/php/7.0/fpm/php-fpm.conf
  * Sites: /etc/php/7.0/fpm/pool.d/\*

**Log/Backup Files**

* /var/log/nginx/
  * Access log - access.log, access.log.X, access.log.X.gz
  * Error log - error.log, error.log.X, error.log.X.gz
* /var/log/supervisor/
  * Django Server: director.log
  * Node.js Server: directornode.log
  * GitHub Deploy Hook: directorgithook.log
* /usr/local/db\_backups
  * Format backupYYYYMMDD.sql.xz
  * Monthly backups of database
  * Run under a crontab with the postgres user

**Other Configuration Files**

* /etc/security/limits.conf
  * @director hard nproc 100 - Limit all sites to a maximum of 100 processes.
  * @director hard as 4000000 - Limit all site processes to a maximum of 4G memory.
* /web
  * NFS share, mounted on ras under /web \# This does not exist atm
  * Kerberos Authentication
  * /etc/exports - NFS Server Configuration

## Installed Software

```text
apt-get install -y sshpass
apt-get install -y htop tmux screen git at
apt-get install -y sqlite3
curl -sL [https://deb.nodesource.com/setup_6.x](https://deb.nodesource.com/setup_6.x) | sudo -E bash -
apt-get install -y nodejs npm
apt-get install -y golang-go
apt-get install -y ruby-full
apt-get install -y php7.0-mbstring php7.0-xml php7.0-mysql php7.0-pgsql php7.0-sqlite php7.0-gd php7.0-ldap php7.0-curl php7.0-mcrypt php7.0-zip composer
apt-get install -y default-jdk default-jre scala
apt-get install -y python-pip python3-pip
apt-get install -y imagemagick
apt-get install -y haskell-platform
apt-get install -y mysql-client
pip install Flask Django requests numpy
pip3 install Flask Django requests numpy
npm install -g pm2
```

* OpenCV
* MediaWiki Parsoid
* [Certbot](https://certbot.eff.org/)

## External Links

* [Director](https://director.tjhsst.edu/)
* [Help Guide](https://director.tjhsst.edu/guide)
* [About/Credits](https://director.tjhsst.edu/about)
* [Director on GitHub](https://github.com/tjcsl/director)

