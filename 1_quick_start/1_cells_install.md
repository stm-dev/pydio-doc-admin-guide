_This section will try to get you up and running in no-time. If you encounter any issue, you may refer to the detailed Installation Guides provided in the next chapter._

## Pre-requisites

#### Hardware/OS

For Pydio Cells to run smoothly, you should meet the following requirements:

* **Server Capacity** : 4GB RAM, 2 CPU/Cores, 64bits processor. SSD is also recommended.
* **Operating System**: Debian 8/9, Ubuntu VERSION, CentOS VERSION, MacOS VERSION, Windows VERSION
* **Ulimit**: Make sure to set the number of allowed open files greater than **2048**. For production use, a minimum of 8192 is recommended (see `ulimit -n`).

#### MySQL Database

You must have an available MySQL database, along with a privileged user (e.g. `pydio`) to access this DB. Use one of the following link to install the DB: 

- [MariaDB version 10.3 and above](https://downloads.mariadb.org/mariadb/repositories)
- [MySQL version 5.7 and above](https://dev.mysql.com/doc/refman/8.0/en/installing.html)

#### Cells binary

Download the cells binary corresponding to your architecture using one of the following links: 

- [Linux Amd64](https://download.pydio.com/latest/cells/release/{latest}/linux-amd64/cells)
- [Mac OSX](https://download.pydio.com/latest/cells/release/{latest}/darwin-amd64/cells)
- [Windows (64bits)](https://download.pydio.com/latest/cells/release/{latest}/windows-amd64/cells.exe)

For cells enterprise, use these links : [Linux Amd64](https://download.pydio.com/latest/cells-enterprise/release/{latest}/linux-amd64/cells-enterprise), [Mac OSX](https://download.pydio.com/latest/cells-enterprise/release/{latest}/darwin-amd64/cells-enterprise), [Windows (64bits)](https://download.pydio.com/latest/cells-enterprise/release/{latest}/windows-amd64/cells-enterprise.exe), and replace `cells` by `cells-enterprise` in all the following commands.

On Linux/MacOSX, make sure to make the binary executable using `chmod +x cells`. Create a dedicated user on the server for the installing and running cells. On Linux, if you wish to start server on ports 80 (http) or 443 (https), you have to grant a proper permission : 
```
setcap 'cap_net_bind_service=+ep' cells
```

## Cells Installation

Start Cells in install mode: 

```
$> ./cells install
```
or on Windows
```
$> .\cells.exe install
```

You will be prompted with the basic networking information for Cells to start its internal webserver : 

- The `Bind URL` (IP address/domain name and port), 
- The `TLS configuration` (to enable HTTPS protocol, strongly recommended!). 
- If you are behind a proxy, you may optionally customize the `External URL` used to access Cells if it differs from your Bind URL.

#### [Mode 1] Web Installer

If you are on a desktop machine, installer can open a simple web page where you will feed the database connection information and default administrator user login and passwords. 

GIF AS IMAGE

[:image:1_quick_start/installation/web-installer.gif]

GIF AS IMAGE-POPUP

[:image-popup:1_quick_start/installation/web-installer.gif]

Once it is finished, the server will restart automatically and your good to go.

#### [Mode 2] Command line Installer

If you are working on a remote server, Cells cannot open a local browser and you can simply perform the same steps using command line prompts/answers :

[:image-popup:1_quick_start/installation/demo-install.gif]

One it is finished, you have to relaunch the server using 

```
$> ./cells start
```
or on Windows
```
$> .\cells.exe start
```

## Login to Cells

Once the restart is finished, you are good to go! You should be able to open your browser at your `External URL` address and see the login page. Use the credentials you just defined to login! 