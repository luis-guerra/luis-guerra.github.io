---
layout: post
title:  "Internet Protocols: HTTP"
date:   2017-10-11 19:08:00 +0200
---

### [](#header-3)About this post
This post will be about the `HTTP protocol`.
I will study how websites behave under certain conditions, analyse HTTP requests and hopefully do some tests with a JSON server.

* * *

### [](#header-3)Obtain info from a website
I decided to go with [WHOis.net](https://www.whois.net) since it's a pretty popular tool to use as a website lookup tool.

The process is very straight-forward. *Just post the URL into the WHOis website and it will tell you its info. Some of that info can be:*

- A domain name (ej. youtube.com)
- A registry domain ID (ej. 142504053_DOMAIN_COM-VRSN)
- The updated / creation date of the site
- Various Domain Status (clientDelete,clientTransfer,clientUpdate,etc...)
- Some Name Servers
- Another (probably) useful info

#### [](#header-4)Examples

##### [](#header-5)google.com:
![googlewhois](https://i.imgur.com/3GAJHxE.png)

##### [](#header-5)twitter.com:
![twitterwhois](https://i.imgur.com/fkNwF9T.png)

* * *

### [](#header-3) Analising HTTP traffic

For this part, I will be using [Charles](https://www.charlesproxy.com). It's a pretty useful tool that allows me to see HTTP traffic, including requests, responses, `HTTP status codes and headers`.

As soon as we grant Charles privilegies, we can start monitoring the HTTP traffic.

#### [](#header-4)Successful Codes
We got a `200` status code as soon as google.com loaded up, that means that `the request has succeeded`:

![charles200](https://i.imgur.com/BNAb5ES.png)

Charles gives us the option to see each of the responses, with their headers and contents, respectively:

![charlesoverview](https://i.imgur.com/VSR1DLs.png)

![charlesdata](https://i.imgur.com/4533sZi.png)

From now on I will be using a website called [getstatuscode](http://getstatuscode.com/200), since it can be pretty hard to get specific status codes only by surfing normally.
Here we have a `204` status code. It means that `the server has fulfilled the request but doesn't need to return a entity/body`:

![charles204](https://i.imgur.com/GoZ40lF.png)

#### [](#header-4)Redirection Codes
Here we have a `301` status code. It means that `the requested URI has been moved permanently`:

![charles301](https://i.imgur.com/CWZvvUC.png)

The `302` status code means that `the requested resource resides temporarily under a different URI`:

![charles302](https://i.imgur.com/QvrG3LR.png)

The `304` status code is returned when `the client performs a conditional GET request and access is allowed, but the document does not get modified`:

![charles304](https://i.imgur.com/pNP4El2.png)

#### [](#header-4)Error codes
We can get a `401` status code when the request requires authentication and this authentication fails:

![charles401](https://i.imgur.com/CkAXT6K.png)

The last status code i'll cover here will be the classic `404`. It means that `the server has not found anything matching the requested URI`:

![charles404](https://i.imgur.com/fFpFdL0.png)

#### [](#header-4)Misc
Here we can see the reddit news page. If I click on the first link, it should redirect me to theguardian.com:

![redditlink](https://i.imgur.com/E6PdcnB.png)

If we click the link we should be able to see which website redirected us to theguardian.com through the HTTP header:

![redditredirect](https://i.imgur.com/XAoFSbs.png)

Unfortunately, I couldn't get the cookies to show on Charles whenever I logged into a website.

* * *
