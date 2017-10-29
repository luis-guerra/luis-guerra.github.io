---
layout: post
title:  "Debian: SSH and Web Server"
date:   2017-10-26 16:13:00 +0200
---

## [](#header-2) About this post
Here I will briefly explain how to configure a `Debian` virtual machine so it serves as a `SSH server` and an `Apache web server`. Administrator access is required for any of the steps below.

* * *

### [](#header-3) SSH Server
To install the SSH server we simply run this command:

```bash
apt-get install openssh-server
```

I should run `apt-get update` before installing the server so the packages are updated.

I should be able to connect to my SSH server now, but first I have to set the VM to `host only mode`, but to be able to do that, I need to create a virtual network within VirtualBox:

![virtualnetwork](https://i.imgur.com/kmtet2Z.png)

![hostonly](https://i.imgur.com/c8GZIxa.png)

To change the default SSH port, just edit the configuration file **/etc/ssh/sshd_config** and change the `Port 22` to whatever you need:

```Shell
nano /etc/ssh/ssh_config
```

![SSHport](https://i.imgur.com/TRbrFHo.png)

Then restart the SSH server with:

```Shell
/etc/init.d/ssh restart
```

* * *

### [](#header-3) Apache2 Server
Installation:

```Shell
apt-get install apache2
```
We should change the VM network configuration to `bridged mode` to be able to connect to the server.

Upon installation, the following directories will be created:
- **/var/www** -> Where our files will be stored and hosted by the server
- **/etc/apache2/** -> Apache configuration file, and other configuration options

Apache organises its configuration tools by directories, such as:

*mods-available:*
> Contains configuration files to both load modules and configure them. The .load files inside this directory contain the Apache Load directives to load the modules into the web server, and the .conf files contain additional configuration directives necessary for the operation of the modules. Modules are enabled using the a2enmod command.

*mods-enabled:*
> Holds symlinks to the files in /etc/apache2/mods-available. When a module configuration file is symlinked, it will be enabled the next time Apache is restarted.

*conf-available:*
> Contains additional configuration files that not associated with a particular module. The configuration files in the conf-available directory are not active unless enabled. To enable a configuration file, the a2enconf command is used, while the a2disconf command is used to disable one.

*conf-enabled:*
> Holds symlinks to the files in /etc/apache2/conf-available. When a configuration file is symlinked, it will be enabled the next time Apache is restarted.

*sites-available:*
> Holds configuration files for Apache Virtual Hosts. Virtual Hosts allow Apache to be configured for multiple sites that have separate configurations. To make a site accessible, a link to its configuration file must be created in the /etc/apache2/sites-enabled directory. This is done using the a2ensite command. To disable a web site, the a2dissite command is used.

*sites-enabled:*
> Contains symlinks to the /etc/apache2/sites-available directory. When a configuration file in sites-available is symlinked, the site configured by it will be active once Apache is restarted.

We can host a website by putting an HTML file under the **/var/www** directory. In this case this file will be called index.html and will only have a phrase:

```Shell
echo "hello world" > /var/www/index.html
```

To change the default location from /var/www we can edit **/etc/apache2/conf/httpd.conf**. In my case i will change it to another directory under /var/www named webs:

Then move the file to the new directory:

```bash
cd /var/www

mkdir webs

mv index.html /webs/index.html
```

If we want to change the default port (80) to another one, we simply edit the **/etc/apache2/ports.conf** file. We can listen to more than one port, if we want so. In this example I will only have the 8080 port:

```
Listen 8080
```

We can also specify IP addresses for the server to listen, with its respective port. Like so:
```
Listen 204.78.6.9:80
```

To disable the apache2 daemon and prevent the web server from starting up with the machine, we can run the following command:

```Shell
update-rc.d -f  apache2 remove
```
