---
layout: post
title:  "Apache Virtual Hosts"
date:   2017-12-03 18:26:00 +0200
---

#### [](#header-3) Apache2 Virtual Hosts

Apache offers us with the ability to use multiple hosts in a single server with Virtual Hosts.

In this example, we'll make it so we have 3 different hosts in our machine.

First, we'll head to `/var/www` in order to create the directories for our hosts. In this case our hosts will be called dam, daw and smx

```bash
mkdir dam
mkdir daw
mkdir smx
```

And we will create a simple file just to test them:

```bash
echo "benvingut/da a DAM" > dam/index.html
echo "benvingut/da a DAW" > daw/index.html
echo "benvingut/da a SMX" > smx/index.html
```

With this, we can begin configuring the virtual hosts.
In order to do this, we will need a configuration file under the directory `sites-available` of the apache configuration:

```bash
cd /etc/apache2/sites-available
```

Here we will create a file for every hosts we want to enable.
To make it simple, will create them from scratch, some people like to copy the `000-default.conf` but that file has many comments and we want to make it as simple as possible:

```bash
touch daw.conf
nano daw.conf
```

Inside the file we can the following contents:

```
<VirtualHost *:80>
  ServerName daw.com
  ServerAlias www.daw.com
  DocumentRoot /var/www/daw
</VirtualHost>
```

And we can copy that file and modify them according to the other directories:

```bash
cp daw.conf dam.conf
nano dam.conf

cp dam.conf smx.conf
nano smx.conf
```

Now that we have the virtual hosts configuration files, we can enable them by running:

```bash
a2ensite dam.conf

a2ensite daw.conf

a2ensite smx.conf
```

To disable a virtual host, all we need to do is go to the `sites-available` directory and run:

```bash
a2dissite virtualHostToDisable.conf
```

To wrap things, we should edit our host machine's host file.
I edited mine to look like this:

![hostsfile](https://i.imgur.com/S0m26FR.png)
