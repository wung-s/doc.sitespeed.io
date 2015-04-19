---
layout: default
title: Docker - Documentation - sitespeed.io
description: Use Docker to run sitespeed.io.
keywords: docker, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Use Docker to run sitespeed.io.
---
[Documentation](/documentation/) / Docker

# Docker
{:.no_toc}

* Lets place the TOC here
{:toc}


## Containers
We have a couple of Docker containers you can use to run sitespeed.io. We have separated them to try to keep the size as small as possible.

 * [The base box](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io-standalone/) - only including sitespeed.io. You can use this if you only want to check web performance best practice rules. And soon when PhantomJS 2 is released for Linux you will be able to fetch timing using Phantom.
 * [Firefox & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io-firefox/) - makes it possible to fetch timings using Firefox. This it the smallest container using a real browser.
 * [Chrome & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io-chrome/) - if you prefer Chrome this is the container to use.
 * [Chrome, Firefox & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io/) - here you get all the things you need, the box gets quite large precisely over 1 gb.


## Running in Docker
The simplest way to run is like this (fetching the box with Chrome and Firefox):

~~~ bash
sudo docker run --privileged --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -u http://www.sitespeed.io -b chrome
~~~

If you want to feed sitespeed with a list of URL:s in a file (here named *myurls.txt*), add the file to your current directory and do like this:

~~~ bash
sudo docker run --privileged --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -f myurls.txt -b chrome --seleniumServer http://127.0.0.1:4444/wd/hub
~~~

## Setup the volume

If you want to feed sitespeed.io with a file with URL:s or if you want the HTML result, you should setup a volume. Sitespeed.io will do all the work inside the container in a directory located */sitespeed.io*. To setup your current working directory add the *-v "$(pwd)":/sitespeed.io* to your parameter list

Note: running on Mac OS X and Windows, Boot2Docker have rights to write data in your */Users* or *C:\Users* directory. Read more [here](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume).
{: .note .note-warning}
