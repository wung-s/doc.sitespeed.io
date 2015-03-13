---
layout: default
title: Dashboard - Documentation - sitespeed.io
description: Web performance dashboard using sitespeed.io.
keywords: dashboard, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Web performance dashboard using sitespeed.io.
---
# Performance Dashboard
{:.no_toc}

* Lets place the TOC here
{:toc}

Coming soon!

<!--
We have put a lot of love into making it easy to create your own performance dashboard.
The base is two Docker images. One with sitespeed.io, Chrome and Firefox. The other image
has Graphite and Grafana.

You can run these images on your own machine or in the cloud. In this example we will use Digital Ocean, just because it they are super fast. Today they have datacenters in San Francisco, New York, London, Amsterdam and Singapore, so you can choose to deploy on one of them or many.

If you are gonna test you site many times, you need to have the $20 instance, we have seen that the Chrome process eats alot of the CPU.

1. Create a new droplet, choose the one with *2 GB / 2 CPUs 40 GB SSD Disk* and the region you want.
2. Click on the *Application* tab and choose *Docker on 14.04*
3. Remember to add the **SSH keys** for your user. Follow the [tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) of how to create your SSH keys.
4. Start your droplet.
5. When it is up and running, log into your server *ssh root@YOUR_IP*
6. Pull the Docker images needed: **docker pull sitespeedio/sitespeeed.io-docker** and
**docker pull sitespeedio/docker-grafana-graphite**
7. Start Grafana & Graphite:

~~~
docker run -d -p 80:80 -p 2003:2003 -p 8125:8125/udp -p 8126:8126 --name sitespeed.io-dashboard sitespeedio/docker-grafana-graphite
~~~
8. **crontab -e** (choose nano)

~~~crontab
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
0,15,30,45 * * * * docker run --privileged --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io-docker sitespeed.io -f urls.txt -b chrome -n 11 --graphiteHost YOUR_IP --memory 256 --connection cable --graphiteNamespace test -r /tmp/ >> /tmp/sitespeed-run.txt
~~~
Make sure to edit your YOUR_IP

-->
