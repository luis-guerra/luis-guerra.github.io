---
layout: post
title:  "Using dig, host and nslookup:"
date:   2018-03-16 16:26:00 +0200
---

### [](#header-3) About these tools:

dig, host and nslookup are *network administration* commands available for Unix machines with the *dnsutils* package.
With these tools we can query `Domain Name Servers (DNS)`.

* * *

#### [](#header-4) Start of Authority record (SOA)

What is a start of authority record?
>Every domain must have a Start of Authority record at the cutover point where the domain is delegated from its parent domain. For example if the domain mycompany.com is delegated to DNSimple name servers, we must include an SOA record for the name mycompany.com in our authoritative DNS records. We add this record automatically for every domain that is added to DNSimple and we show this record to you as a System Record in your domain’s Manage page.

A SOA record includes the following details:
- The primary name server for the domain, which is ns1.dnsimple.com or the first name server in the vanity name server list for vanity name servers.
- The responsible party for the domain, which is admin.dnsimple.com.
- A timestamp that changes whenever you update your domain.
- The number of seconds before the zone should be refreshed.
- The number of seconds before a failed refresh should be retried.
- The upper limit in seconds before a zone is considered no longer authoritative.
- The negative result TTL (for example, how long a resolver should consider a negative result for a subdomain to be valid before retrying).

We can query SOA record for the domain *'mataro.escolapia.cat'* like this:

**nslookup:**
```Shell
nslookup -type=soa mataro.escolapia.cat
```

**host:**
```Shell
host -t soa mataro.escolapia.cat
```

**dig:**
```Shell
dig soa mataro.escolapia.cat
```

**Results(nslookup):**
![soa](https://i.imgur.com/n7lJiJk.png)

#### [](#header-4) NS records

What is a NS record?:
>An NS record is used to delegate a subdomain to a set of name servers. Whenever you delegate a domain to DNSimple the TLD authorities place NS records for your domain in the TLD name servers pointing to us.

We can query the NS record for the domain *mataro.escolapia.cat* like this:

**nslookup**
```Shell
nslookup -type=ns mataro.escolapia.cat
```

**host**
```Shell
host -t ns mataro.escolapia.cat
```

**dig**
```Shell
dig ns mataro.escolapia.cat
```

**Results(dig):**
![ns](https://i.imgur.com/o00p9Iy.png)

#### [](#header-4) MX record

What are MX records?
>MX stands for Mail eXchange. MX Records tell email delivery agents where they should deliver your email. You can have many MX records for a domain, providing a way to have redundancy and ensure that email will always be delivered.

We can query MX records for the domain *marca.com* like this:
**nslookup**
```Shell
nslookup -type=mx marca.com
```

**dig**
```Shell
dig mx marca.com
```

**Results(dig):**
![mx](https://i.imgur.com/CTxyI2j.png)

#### [](#header-4) A Record

What is an A record?
>An A record maps a domain name to the IP address (IPv4) of the computer hosting the domain. Simply put, an A record is used to find the IP address of a computer connected to the internet from a name.
The A in A record stands for Address. Whenever you visit a web site, send an email, connect to Twitter or Facebook or do almost anything on the Internet, the address you enter is a series of words connected with dots.

Query A records for the domain *facebook.es* like this:

**nslookup**
```Shell
nslookup -all facebook.es
```

**host**
```Shell
host -a facebook.es
```

**dig**
```Shell
dig a facebook.es
```

**Results(host)**
![a](https://i.imgur.com/Po8f3sV.png)

#### [](#header-4) Pointer Records (PTR)

What are pointer records?
>Pointer records are used to map a network interface (IP) to a host name. These are primarily used for reverse DNS.

A pointer record has the following details:

- *Name:* This usually represents the last octet of the IP address.
- *System (PTR to):* This will be the value (the reverse DNS) for your host / computer within your domain.
- *TTL:* The TTL (Time to Live) is the amount of time your record will stay in cache on systems requesting your record (resolving nameservers, browsers, etc.). The TTL is set in seconds, so 60 is one minute, 1800 is 30 minutes, etc.

How to query pointer records?:
In this case, the ip I'll be using *facebook.es' IP (1.13.83.8)*

**nslookup**
```Shell
nslookup -type=ptr 1.13.83.8
```

**host**
```Shell
host 1.13.83.8
```

**dig**
```Shell
dig -x 1.13.83.8
```

**Results(dig)**
![ptr](https://i.imgur.com/NfgSNpM.png)

#### [](#header-4) CNAME record

What is a CNAME record?
>CNAME stands for Canonical Name. CNAME records can be used to alias one name to another.

We can query CNAME records for the domain *www.facebook.es* like this:

**nslookup**
```Shell
nslookup -type=cname www.facebook.es
```

**host**
```Shell
host -t cname www.facebook.es
```

**dig**
```Shell
dig cname www.facebook.es
```

**Results(nslookup and host)**
![cname1](https://i.imgur.com/9Y6XOzi.png)
![cname2](https://i.imgur.com/U8DV45Y.png)

#### [](#header-4) dig + trace

How does it work?
>dig +trace works by pretending it's a name server and works down the namespace tree using iterative queries starting at the root of the tree, following referrals along the way.

The exact process is documented in [this article by developer HUB](https://ns1.com/articles/using-dig-trace)

We can try it out like this:

```Shell
dig +trace microsoft.com
```

**Result:**
![trace](https://i.imgur.com/WvH8CmZ.png)
