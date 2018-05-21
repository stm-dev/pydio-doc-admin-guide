_This guide describes the steps required to have Pydio Cells running on macOS._

[:image-popup:1_installation_guides/logos-os/logo-mac.png]

## Requirements

### PHP

PHP FPM is required to run the Pydio frontend. To install, run the following commands:

```sh
brew tap homebrew/dupes
brew tap homebrew/versions
brew tap homebrew/homebrew-php
brew install php72 php72-intl
```

### Database 
*you can skip this step if you already have a database.*

You can use either MySQL (>= 5.6) or MariaDB as your Database Management System. Both are available in Homebrew.

```brew install mysql```
or
```brew install mariadb```

## Installation

### Pydio

Download Pydio Cells Binary on your server/machine using the following command :

```sh
wget https://download.pydio.com/pub/cells/release/0.9.0/darwin-amd64/cells
chmod +x cells
```

### Port 80 & 443

You can only use these ports if you are connected as root.

By default, Apache is running on macOS, so you need to ensure that it - or no other webservers - is bound to these ports.

To stop the default Apache, you can use :

```sudo apachectl stop```

To prevent Apache from starting during launch, you may use :

```sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist```

### Database configuration

In this section, we assume you have installed MySql server. Adapt the following steps to your current installation.

```sh
# Go to mysql mode
sudo mysql -u root
# Create new user and set password
CREATE USER 'pydio'@'localhost' IDENTIFIED BY 'your password goes here';
```

## Starting with Pydio

First, give execution rights to the binary. For instance, you can use `sudo chmod u+x <binary>`.

Then, to launch the installer, type:

```sh
./cells install
```

You can [refer to this page](/en/docs/cells/v1/install-pydio-cells) to get more details on the installation process.

After the install is successfully done, if you ever have to stop Pydio Cells and want to run it again, just run:

```sh
./cells start
```

## Troubleshooting

* The php-fpm service might not be started, you can look at its status using : `brew services list` and then `brew services start php72` if needed.

* The database ervice might not be started, you can look at its status using : `brew services list` and then `brew services start mysql` if needed.

* You can look at the webserver's error file located in `~/.config/pydio/cells/logs/caddy_errors.log`.