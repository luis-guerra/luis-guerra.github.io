---
layout: post
title:  "Internet Protocols: HTTP"
date:   2017-10-11 19:08:00 +0200
---

###About this post
This post will be about the `HTTP protocol`.
I will study how websites behave under certain conditions, analyse HTTP requests and hopefully do some tests with a JSON server.

* * *

###Obtain info from a website
I decided to go with [WHOis.net](https://www.whois.net) since it's a pretty popular tool to use as a website lookup tool.

The process is very straight-forward. *Just post the URL into the WHOis website and it will tell you its info. Some of that info can be:*

- A domain name (ej. youtube.com)
- A registry domain ID (ej. 142504053_DOMAIN_COM-VRSN)
- The updated / creation date of the site
- Various Domain Status (clientDelete,clientTransfer,clientUpdate,etc...)
- Some Name Servers
- Another (probably) useful info

####Examples

#####google.com:
[!googlewhois](https://i.imgur.com/3GAJHxE.png)

#####twitter.com:
[!twitterwhois](https://i.imgur.com/fkNwF9T.png)
