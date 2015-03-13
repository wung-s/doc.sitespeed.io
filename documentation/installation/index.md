---
layout: default
title: Installation - Documentation - sitespeed.io
description: How to to install sitespeed.io. Use npm, our Vagrant files or Docker.
keywords: installation, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: How to to install sitespeed.io. Use npm, our Vagrant files or Docker.
---
[Documentation](/documentation/) / Installation

# Installation
{:.no_toc}

* Lets place the TOC here
{:toc}

# Download and installation

## Install on Mac, Linux & Windows

Prerequisites: Install [NodeJS](http://nodejs.org/download/) ([Linux](https://github.com/creationix/nvm)) and make sure you have [npm](https://github.com/npm/npm) installed.

~~~ bash
$ npm install sitespeed.io
~~~

If you want it installed globally (running it from wherever):

~~~ bash
$ npm install -g sitespeed.io
~~~

Run

~~~ bash
sitespeed.io -h
~~~

or on Windows:

~~~ bash
$ sitespeed.io.cmd -h
~~~

## Vagrant

We have a couple of [Vagrant](https://github.com/sitespeedio/sitespeed.io-vagrant) boxes to get you up and running fast (installing sitespeed.io and browsers).

* [Ubuntu 14](https://github.com/sitespeedio/sitespeed.io-vagrant/tree/master/sitespeed-ubuntu14)
* [CentOS 7](https://github.com/sitespeedio/sitespeed.io-vagrant/tree/master/sitespeed-centos7)

## Docker

We have [Docker images](https://registry.hub.docker.com/repos/sitespeedio/) with sitespeed.io, Chrome, Firefox and Xvfb. They are super easy to use (Xvfb is started automatically when you start the container). Here's how to use the container with both Firefox & Chrome (install [Docker](https://docs.docker.com/installation/ubuntulinux/) or [Boot2Docker](https://github.com/boot2docker/osx-installer) first and start them).

~~~ bash
$ sudo docker pull sitespeedio/sitespeed.io
$ sudo docker run --privileged --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -u http://www.sitespeed.io -b firefox
~~~

That will output the data from the run in the current directory.

Note: running on Mac OS X and Windows, Boot2Docker have rights to write data in your */Users* or *C:\Users* directory. Read more [here](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume).
{: .note .note-warning}