---
layout: post
title:  "Application Containers & Virtualization Boxes"
date:   2018-03-03 18:18:20 +0200
---

### [](#header-3) About this Post

In this post we'll cover [Docker](https://www.docker.com), an application that isolates dependencies into a "container" and [Vagrant](https://www.vagrantup.com), that allows us to manage virtual machine environments as "boxes".
Both of these tools allows us to have multiple application/services work together while also being independent of one another.

### [](#header-3) Deploying and Uploading a Docker Container

I created a directory on my host machine as it will be the source for the Docker container:

![web](https://i.imgur.com/O71VOMS.png)

To install docker, I used Homebrew:

```bash
brew install docker
```

We'll need a `DockerFile` to deploy a container. A DockerFile is a specific configuration for a given container:

```bash
touch Dockerfile


/* Dockerfile */

FROM php:7.1-apache
COPY  web/  /var/www/html/
EXPOSE 80
```

We can now build the docker container with the `build` command:

```bash
docker build -t mi-docker
```

To execute the container as a `daemon`, run:

```bash
docker run -d -p 80:80 mi-docker
```

To stop the container, run:

```bash
docker stop $(docker ps -q --filter ancestor=<image-name> )
```

To upload a container to Docker Hub, we will head to [Docker Hub](hub.docker.com) and create a new repository for our container:

![repo](https://i.imgur.com/Jy4JXfc.png)

 Then login via the command line:

```bash
docker login
```

And create the container

```bash
docker create mi-docker mi-container
```

Once done, we can now push the container to our hub repository:

```bash
docker push luisguerr4/test
```

We should be able to see the container in our repository on Docker Hub.

### [](#header-3) Deploying a Vagrant Box

To deploy a Vagrant Box, we need to make sure to have already `installed a virtualization software`, like VMWare or Virtual Box.

First, we'll need to decide which box we want to use. A box is a term for an already configured virtual machine.
In this case I'll use [Scotch Box](https://box.scotch.io), which is a box that includes powerful dependencies like:

- PHP 7.0
- MySQL 5.7
- Go
- Yarn
- Node
- **and many more**

We add Scotch box to our vagrant default boxes:
```bash
vagrant add scotch/box
```

Scotch box offers a git repository to get started with the box.:
```bash
git clone https://github.com/scotch-io/scotch-box
```

And start the machine by running
```bash
vagrant up
```

We can now visit http://192.168.33.10 to see the box's homepage.

To stop it, we simply run
```bash
vagrant halt
```

To see the status of the current vagrant environment, run
```bash
vagrant status
```

Any Vagrant environment has a `Vagrantfile`, which contains the configuration of the current box.

We can also log into the box via ssh with the command:
```bash
vagrant ssh
```

The credentials vary depending on the box. But you should be able to find them in their box homepage.
