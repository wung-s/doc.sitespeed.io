---
layout: default
title: Continous integration - Documentation - sitespeed.io
description: How to configure sitespeed.io
keywords: configuration, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Configuration for the sitespeed.io.
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

We have Docker [images](https://registry.hub.docker.com/repos/sitespeedio/) with sitespeed.io, Chrome, Firefox and Xvfb.
