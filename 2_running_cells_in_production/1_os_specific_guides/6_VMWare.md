The Pydio Cells image for VMWare is based on CentOS 7. It has been enriched with necessary third party software and pre-configured to provide an easy to run instance of the Cells server out of the box.  
It is known to run in VSphere echosystems and in standalone ESXi hosts.

## Download the package

The latest image for VMWare can be [Downloaded Here](https://download.pydio.com/latest/cells-enterprise/release/{latest}/vmware/Cells-Enterprise-VMWare-{latest}.zip).

An md5 file is also available on the same location for integrity verification.

## Launching a VM

In this guide, we use the simple standalone ESXi web interface to launch a new virtual machine.

**Virtual Machine requirements:**

- RAM : 4G
- CPU : 2 vCores
- CPU support: VT-x/AMD-V
- NICs: bridged adaptor

### Setup and config at the first time

To create a new VM from the image:

- Log in your web interface
- Go to the `Virtual Machines` section
- Click on Create/Register VM

A creation wizard pops up.

- In `Select creation type`, choose `Deploy a virtual machine from an OVF or OVA file`
- In second page, choose a name and upload **both** the `*.ovf` and the  `*-disk1.vmdk` files
- Choose correct network and disk provisioning
- Click on `Finish` and wait until creation is complete

At first boot, a script is launched to verify network settings and start the installer. You can then access the installation wizard with a web browser to configure your instance.

For your convenience, we display (in the VM console accessed from your ESXi GUI) the full correct URL you have to use to access this post installation configuration wizard - formatted as `https://<DN or IP>/`.

*Note: A self-signed certificate is used by default, ignore the warning message on your web browser*.

Just follow the steps of the wizard to finalize your setup. All parameters are set by default except the main administrator password. Upon termination, the installer saves everything, starts the `cells` service and exits. It can take up to a minute to reach the end.

At this step, you can log in the app with the credentials you have just entered and verify everything is up and running.

### Predefined accounts

If you ever need to log into the VM via terminal, users are created as follow:

| user                | username        | password    |
| ------------------- | --------------- | ----------- |
| administrative user | root            | cells       |
| user                | pydio           | cells       |
| MySQL root user     | root            | no password |
| MySQL user          | pydio@localhost | no password |

The predefined database created in MySQL is *cells*

## Notes on configuration

By default, two root paths are used:

- `/var/cells`: Pydio Cells working dir. It contains dynamic configuration (including certificates), data and logs.
- `/opt/pydio`: binaries and additional libraries required to run Cells.

### Use of well-known ports

In order for Cells to be able to use well known 80 & 443 port, you have to give specific permissions to the binary file.
The OVF you have downloaded is correctly configured and the embedded binary has already these permission set. When upgrading, the binary changes, but the permissions are automatically re-applied by systemd upon restart.

Yet, if you ever need to manually apply these permissions on the binary file:

```sh
# log as pydio via ssh into the machine
sudo setcap 'cap_net_bind_service=+ep' /opt/pydio/cells
```

### Systemd service

**cells** service is running under **pydio** user. The service is **enabled** (a.k.a will automatically restart at reboot).

To manually restart Cells:

```sh
# As pydio user
sudo systemctl restart cells
```

You can consult the output of **cells** service by using command:

```sh
sudo journalctl -f
# or to see only cells related log:
sudo journalctl -f -u cells
```

To start/stop the database (MariaDB):

```sh
sudo systemctl start mariadb
```

### Firewalld service

Firewalld service is active and opens three ports:

- 80: **cells-enterprise**
- 443: **cells-enterprise**
- 22: **ssh**

### SELinux

SELinux is running in *permissive* mode.
