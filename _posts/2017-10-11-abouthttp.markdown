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

**Charles gives us the option to see each of the responses, with their headers and contents, respectively:**

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

### [](#header-3)HTTP Request Methods

For this part, we will install a JSON server via NPM with the following command:
```bash
npm install -g json-server
```
![jsonshell](https://i.imgur.com/FQf6dXp.png)

I will also use [Postman](https://www.getpostman.com) in order to monitor the API REST.
As we can see, the JSON server was installed in `/usr/local/lib/node_modules/json-server`, if we go to that directory we can find a file called `db.json`

![dbjson](https://i.imgur.com/H7mlmED.png)

I will add some data to simulate a "real" enviroment:

```json
{

  "clientes": [

    { "id": 1, "nombre": "Juan Marin Gomez", "telefono": "666111111" },

    { "id": 2, "nombre": "Pedro Sanz Camas", "telefono": "666222222" },

    { "id": 3, "nombre": "Toni Casas Ramirez", "telefono": "666333333" }

  ]

}
```

We can start the JSON server with the following command (you must be on the JSON-server directory):

```bash
json-server --watch db.json
```

![jsonserverstart](https://i.imgur.com/ylApjNd.png)

Now we can start requesting with Postman:

#### [](#header-4)GET:

`GET` is used to retrieve information from the given server using a given URI.

As we can see, the JSON server is working correctly at my `localhost:3000`:

![GET](https://i.imgur.com/KRwxEyd.png)

Postman also gives us the ability to see the `request headers`:

![GETHEADERS](https://i.imgur.com/p8BcLQP.png)

We can access the JSON server `resources` adding `/clientes` to the URI:

![GETDATA](https://i.imgur.com/Qy5YjpN.png)

#### [](#header-4)HEAD:

`HEAD` works the same as `GET` but it only transfers the status line and the header section.

![HEAD](https://i.imgur.com/JBeotgp.png)

#### [](#header-4)OPTIONS:

`OPTIONS` is used to describe the communications options for the target resource.

![OPTIONS](https://i.imgur.com/XsCBB8T.png)

#### [](#header-4)POST:

The `POST` request is used to send data to the server.

In this case i will add more data to the JSON server:

![POST1](https://i.imgur.com/9HVLTHD.png)

As we can see, if we do a GET request we posted the data successfully:

![POST2](https://i.imgur.com/njOuAhE.png)

#### [](#header-4)PUT:

The `PUT` method replaces all the current representations of the target resource with the upload content.

In this case, the target resource is `localhost:3000/clientes/1` since we want to modify the first entry:

![PUT1](https://i.imgur.com/7zNUbMc.png)

Now we can check the whole JSON:

![PUT2](https://i.imgur.com/EOAOCeH.png)

#### [](#header-4)DELETE:

`DELETE` removes the current representation of the target resource.

In this case we will remove the third entry in out JSON:

![DELETE1](https://i.imgur.com/WoAN5jo.png)

As we can see, the entry was successfully deleted:

![DELETE2](https://i.imgur.com/xXXVbor.png)

Finally, we can see the transactions on the JSON server (although i made these tests at different times so it doesn't show all the requests):

![log](https://i.imgur.com/SLP67Xq.png)


`this is a test`:

>what the fuck harry? what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?what the fuck harry?
