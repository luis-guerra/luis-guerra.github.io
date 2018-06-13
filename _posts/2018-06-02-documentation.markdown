---
layout: post
title:  "Installation and Configuration of OpenLDAP"
date:   2018-06-02 16:26:00 +0200
---

### [](#header-3) OpenLDAP
OpenLDAP is a free, open source implementation of the Lightweight Directory Access Protocol.
First, we'll install slapd and ldap-utils:
```bash
apt-get install slapd ldap-utils
```

Once done, we'll run the following command in order to reconfigure slapd:
```bash
sudo dpkg-reconfigure slapd
```
You will be asked a series of questions about how you'd like to configure the software.
Everything is up to you, although in this case we'll call the DNS **m8dawuf3.net** and the organisation name **m8dawuf3**
You should move the old database as well.

Now we are ready to create **organisational units**:
We'll make a file called **ou.ldif** and add the following:
```bash
dn: au=administrators,dc=m8dawuf3,dc=net
out: administrators
objectClass: organisationalUnit

dn: au=desenvolupadors,dc=m8dawuf3,dc=net
out: desenvolupadors
objectClass: organisationalUnit
```

Now that we've created the organisational unit, we need to stop the slapd service to add it:
```bash
service slapd stop
```

And add the organisational unit:
```bash
slapadd -c -v -l ou.ldif
```

To add users to this organisational unit, you'll need to create another file with the **.ldif** extension. In this case our file is called user.ldif:
![ldap1](https://i.imgur.com/JlYunIL.png)

To add it, we'll run the **ldapadd** command:
```bash
ldapadd -h localhost -c -x -D *cn=admin,dc=m8dawuf3,dc=net* -W -f user.ldif
```

**Searching the ldap database**:
```bash
ldapsearch -u -x -b 'o=TUDelft,dc=net' 'objectclass=*'
```

The -b option stands for searchbase (initial search point), the -u option stands for userfriendly output information and the -x option is used to specify simple authentication.

**Removing from the ldap database**:
```bash
ldapdelete 'cn=admin,o=TUDelft,dc=net'
```

**Modifying from the ldap database**:
Assuming that the file /tmp/entrymods exists and has the contents:
```bash
dn: cn=Modify Me, o=Some Unit, c=net
changetype: modify
replace: mail
mail: modme@terminator.rs.itd.umich.edu
-
add: title
title: Grand Poobah
-
add: jpegPhoto
jpegPhoto: /tmp/modme.jpeg
-
delete: description
-
```

We'll run:
```bash
ldapmodify -b -r -f /tmp/entrymods
```

To replace the contents of the "Modify Me" entry's mail attribute with the value "modme@terminator.rs.itd.umich.edu", add a title of "Grand Poobah", and the contents of the file /tmp/modme.jpeg as a jpegPhoto, and completely remove the description attribute.
