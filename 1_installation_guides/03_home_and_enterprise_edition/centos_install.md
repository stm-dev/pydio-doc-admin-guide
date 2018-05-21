_This guide describes the steps required to have Pydio Cells running on a CentOs/RHEL 7 server._

[:image-popup:1_installation_guides/logos-os/logo-centos.png]

## Prerequisites 

### Repositories

The version of packages such as PHP or MySQL (MariaDB) is outdated by default. We need to use extra repositories with more recent versions.

#### EPEL release

```sh
sudo yum install epel-release scl-utils
```

#### Software collection release

CentOS:

```sh
sudo yum install centos-release-scl
```

RedHat:

```sh
sudo yum-config-manager --enable rhel-server-rhscl-7-rpms
```

### Database

You can either use MySQL or MariaDB.

#### MySQL

Install MySQL 5.6 official community release repository.

```bash
sudo rpm -i http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
sudo yum update

# install mysql-community-server package
sudo yum install mysql-community-server

# Set mysqld to start after reboot
sudo systemctl enable mysqld

# start the service now
sudo systemctl start mysqld
```

#### MariaDB

To install MariaDB, follow below steps

```sh
# install the RPM
sudo yum install rh-mariadb102-mariadb-server
# start MariaDB after reboot
sudo systemctl enable rh-mariadb102-mariadb
# start the service
sudo systemctl start rh-mariadb102-mariadb
```

#### Post install configuration

By default, a new database will be created by the system during the installation process. You only need a user with database management permissions.

If you would rather do it manually, you may create a dedicated user and an empty database by executing the following SQL queries :

```SQL
CREATE USER 'pydio'@'localhost' IDENTIFIED BY '<your-password-here>';
CREATE DATABASE cells;
GRANT ALL PRIVILEGES ON cells.* to 'pydio'@'localhost';
FLUSH PRIVILEGES;
```

### SELinux

There is no available configuration of SELinux for Pydio Cells. Please make sure that SELinux is disabled or running in permissive mode.

To temporary disable SELinux: `sudo setenforce 0`.

You can also permanently disable SELinux in `/etc/selinux/config`.

### PHP-FPM

In this example we use PHP version 7.1, but you can use any version >= 5.5.9.

```bash
sudo yum install rh-php71-php-fpm rh-php71-php-common rh-php71-php-intl rh-php71-php-gd rh-php71-php-mbstring rh-php71-php-xml rh-php71-php-curl rh-php71-php-opcache
```

The default **user** for the PHP-FPM worker pool needs to be changed to **pydio**. Change the port the service is listening to if required.

```bash
sudo vi /etc/opt/rh/rh-php71/php-fpm.d/www.conf

; ... omitted

; Unix user/group of processes
; RPM: apache user chosen to provide access to the same directories as httpd
; user = apache
user = pydio
; RPM: Keep a group allowed to write in log dir.
group = apache

listen = 127.0.0.1:9000
```

Then enable and start the PHP-FPM service:

```bash
sudo systemctl enable rh-php71-php-fpm
sudo systemctl start rh-php71-php-fpm
```

### Dedicated User

It is recommended to use a dedicated user to run Pydio Cells.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
sudo passwd pydio
```

Switch to this user to run the installation

```sh
su - pydio
```

## Install Pydio Cells

```sh
wget https://download.pydio.com/pub/cells/release/1.0.0/linux-amd64/cells
chmod u+x cells
# if you need to use the standard http (80) or https (443) port, please execute this command:
setcap 'cap_net_bind_service=+ep' cells
./cells install
```

Follow the short set of instructions to finish off the Pydio Cells installation. You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details.

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

### Frontend

#### SELinux is enforced

If, after a successful installation and when you try to navigate to the main application page with your browser, you land on a blank page with following message:

> Access denied.

Insure you have modified SELinux to be in permissive mode.