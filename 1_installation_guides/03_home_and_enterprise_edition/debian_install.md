
_This guide describes the steps required to have Pydio Cells running on Debian 8 and Debian 9._

[:image-popup:1_installation_guides/logos-os/logo-debian.png]

## Requirements

### OS requirements

You need the 64-bit version of one of these Debian or Raspbian versions:

- Stretch 9 (stable) / Raspbian Stretch
- Jessie 8 (LTS) / Raspbian Jessie

#### Dedicated User

It's highly recommend to run Pydio Cells with a dedicated user.

In this guide, we use **pydio** and its home directory **/home/pydio**.

In order to create a new user and its home directory execute this command:

```sh
sudo useradd -m pydio
```

### Database

Pydio Cells can be installed with both MySQL Server (v5.6 or higher) and MariaDB (v10.1 or higher).

#### MySQL

##### Repository

```bash
wget https://repo.mysql.com/mysql-apt-config_0.8.9-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.9-1_all.deb
```

To access the correct repository :

- Select the `MySQL Server & Cluster` option and press `Enter`
- Select the correct option (`mysql-5.6` or `mysql-5.7`) and press `Enter`
- Back on the first list, select `Ok` and press `Enter`

##### Install

```bash
sudo apt update
sudo apt install mysql-server
```

#### MariaDB

##### Repositories

###### Debian 8

``` bash
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
sudo add-apt-repository 'deb [arch=amd64,i386,ppc64el] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian jessie main'
```

###### Debian 9

``` bash
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64] http://www.ftp.saix.net/DB/mariadb/repo/10.1/debian stretch main'
```

##### Install

```sh
sudo apt update
sudo apt install mariadb-server
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

### PHP

- **[DEBIAN 8 ONLY]  Add the PHP 7 repository**
    PHP 7 package are not available from official package list, so you need to add them.
    
    ```sh
    echo "deb http://packages.dotdeb.org jessie all" | sudo tee -a /etc/apt/sources.list.d/dotdeb.list
    wget -O- https://www.dotdeb.org/dotdeb.gpg | sudo apt-key add
    sudo apt update
    ```

- **Install**

```sh
sudo apt install php7.0 php7.0-fpm php7.0-gd php7.0-curl php7.0-intl php7.0-xml
```

- **FPM configuration**

    Pydio can communicate with fpm though TCP socket or UNIX socket. You have to edit the fpm config file and tell whether to listen on TCP port 9000 or to listen on UNIX socket at /var/run/php7-fpm.sock. Here are steps to do so.

    #### TCP Socket

    You simply have to add this directive ***listen = 9000*** to the fpm configs file

    ```sh
    # Note that you must execute the *2* following lines at a time.
    sudo sed -i '$ a\
    listen=9000' /etc/php/7.0/fpm/php-fpm.conf
    ```

    #### UNIX Socket

    You have to insure the user that runs the Pydio Cells binary has sufficient rights on the socket.
    You have many options, we usually add the corresponding user to the default `www-data` group and change the `listen.owner` directive of the fpm configuration file.
    
    So edit the `/etc/php/${PHP_VERSION}/fpm/pool.d/www.conf` configuration file to have something like:

    ```sh
    ...(content omitted)...
    #listen.owner= <the_correct_user>
    listen.owner= pydio
    listen.group= www-data
    ...(content omitted)...
    ```
    Then, add the pydio user to www-data group and add write permission to the www-data group to the php folder:
    ```sh
    # as *root* user
    # addgroup <the_correct_user> www-data, for instance:
    addgroup pydio www-data
    chmod g+w /run/php
    ```

    Note: if you were logged in as user `pydio` when you did `su -`, you have to log out and back in for the permission update to be effective.

- **Finalisation**

    Enable and restart PHP FPM service to apply the changes:

    ```sh
    sudo systemctl enable php7.0-fpm
    sudo systemctl restart php7.0-fpm
    ```

## Installation and configuration

```sh
wget https://download.pydio.com/pub/cells/release/1.0.0/linux-amd64/cells
sudo chmod u+x cells
```

If you need to use the standard http (80) or https (443) port, please execute this command:

```sh
setcap 'cap_net_bind_service=+ep' cells
```

Switch to the **pydio** user to run the installation and start the app:

```sh
su - pydio
```

Execute the command below and follow the instructions.

```sh
./cells install
```

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again just run:

```sh
./cells start
```

## Troubleshooting

Generally, you might want to have a look at the log file that is located in `~/.config/pydio/cells/logs`.

### FPM and Websocket issues

_You have installed and started Pydio Cells with no problem, but when you connect, you only see a blank page with `File not found.` message._

It's usually related to a misconfiguration of the websocket, you should check the following

#### Is the php-fpm service running?

To check service status, do:

```sh
sudo systemctl status php<version>-fpm

# Enable and start it, if necessary
sudo systemctl enable php<version>-fpm
sudo systemctl start php<version>-fpm
```

#### Is the socket correctly configured?

If you are using the TCP socket, you might have forgot to add `listen = 9000` to the php-fpm.conf file.

For the UNIX socket users, you might have forgot to change the `listen.owner` or `listen.group` directives located in the `<php-fpm path>/pool.d/www.conf` file.

### Database server issue

_After start, the web page is unreachable and you see a bunch of errors starting with: `ERROR	pydio.grpc.meta	Failed to init DB provider	{"error": "Error 1071: Specified key was too long; max key length is 767 bytes handling data_meta_0.1.sql"}`._

You might have an unsupported version of the mysql server: you should use MySQL server version 5.6 or higher or MariaDB version 10.2 or higher. 

### Various

_You see this error: `/lib/x86_64-linux-gnu/libc.so.6: version 'GLIBC_2.14' not found`_

The version of libc6 is outdated. Run these commands to upgrade it.

```sh
sudo apt-get update
sudo apt-get install libc6
```