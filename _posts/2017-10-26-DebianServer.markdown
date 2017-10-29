---
layout: post
title:  "Debian: SSH and Web Server"
date:   2017-10-26 16:13:00 +0200
---

## [](#header-2) About this post
Here I will briefly configure a `Debian` machine so it serves as a `SSH server` and an `Apache web server`. Administrator access is required for any of the steps below.

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

To change the default SSH port, I need to change the configuration file `/etc/ssh/sshd_config` and change the `Port 22` to whatever I need.

![SSHport](https://i.imgur.com/TRbrFHo.png)

Now we restart the SSH server with:

```bash
/etc/init.d/ssh restart

```

### [](#header-3) Apache2 Server
Installation:

```bash
apt-get install apache2
```

We should change the VM network configuration to `bridged mode` to be able to connect to the server:

![bridgedmode]()

There are several useful directories that makes Apache2 a good and versatile web server:

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

We can host a website by putting an HTML file under the `/var/www` directory. In this case this file will be called index.html and will only have a word:

```bash
echo "hello world" > /var/www/index.html
```

To change the default location from /var/www we can edit `/etc/apache2/conf/httpd.conf`. In my case i will change it to another directory under /var/www named webs:

![webs]()

![httpd]()

We can see the changes if I move the index.html to the new directory:
```bash
mv index.html /webs
```



daw08
daw08
admin
https://wiki.debian.org/Apache
https://stackoverflow.com/questions/5891802/how-do-i-change-the-root-directory-of-an-apache-server
https://askubuntu.com/questions/170640/how-to-disable-apache2-server-from-auto-starting-on-boot
https://www.cyberciti.biz/faq/linux-apache2-change-default-port-ipbinding/
