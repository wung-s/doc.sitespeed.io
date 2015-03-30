---
layout: default
title: Dashboard - Documentation - sitespeed.io
description: Web performance dashboard using sitespeed.io.
keywords: dashboard, docker, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Web performance dashboard using sitespeed.io.
---
# Performance Dashboard
{:.no_toc}

* Lets place the TOC here
{:toc}


## How it works
We have put a lot of love into making it easy to create your own performance dashboard. To get it up and runing you need [Docker](https://www.docker.com/). That's it :)

The base is these three Docker images:

  * [Sitespeed.io with Chrome, Firefox & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io/).
  * [Graphite that stores the collected metrics](https://registry.hub.docker.com/u/sitespeedio/graphite/). If you have an Graphite server up and running already you can use that one.
  * [The official Grafana image, making it possible to create nice graphs of your metrics](https://registry.hub.docker.com/u/grafana/grafana/).

You can run these images on your own machine(s) or in the cloud. You only need Docker. But what will you get? We have setup an example site so you can try it out yourself. We are proud to present
[dashboard.sitespeed.io](http://dashboard.sitespeed.io:3000/dashboard/db/american-airlines-summary).

## Setup the containers

It is extremely easy to setup the containers. The only thing you need to do is setup directories where you store the data.

### Graphite
First we want to have have Graphite to store the metrics. You want to store the data outside of your containers, so create an directory where you store the data. In this example we put it in */data/graphite*

`sudo mkdir -p /data/graphite/storage/whisper`.

Then start the image.

~~~
sudo docker run -d \
  --name graphite \
  -p 8080:80 \
  -p 2003:2003 \
  --restart="always" \
  -v /data/graphite/storage/whisper:/opt/graphite/storage/whisper  \
  sitespeedio/graphite
~~~

Your Graphite instance will be behind Basic Auth (*guest*/*guest*), if your server is public you should change that by generating your own .httpwd file. You can do that with [apache2-utils](http://httpd.apache.org/docs/2.2/programs/htpasswd.html). You run it like this:

~~~
sudo apt-get install apache2-utils
sudo htpasswd -c .htpasswd YOUR_USERNAME
~~~

And add this line when you start the Graphite docker image:

~~~
-v /your/path/.htpasswd:/etc/nginx/.htpasswd \
~~~

The full startup would then look like this

~~~
sudo docker run -d \
  --name graphite \
  -p 8080:80 \
  -p 2003:2003 \
  --restart="always" \
  -v /data/graphite/storage/whisper:/opt/graphite/storage/whisper \
  -v /your/path/.htpasswd:/etc/nginx/.htpasswd \
  sitespeedio/graphite
~~~


#### Configure how long to store the metrics
By default the metrics are stored for 60 days and you can change that. First [read]((https://github.com/etsy/statsd/blob/master/docs/graphite.md)) Etsy:s nice writeup on how you configure Graphite. Create your own [storage-schemas.conf](https://github.com/sitespeedio/docker-graphite-statsd/blob/master/conf/graphite/storage-schemas.conf) file and feed it to the image on startup like this:

~~~
-v /path/to/storage-schemas.conf:/opt/graphite/conf/storage-schemas.conf
~~~


### Grafana
Start Grafana

~~~
sudo docker run -d -p 3000:3000 \
--name grafana \
--restart="always" \
grafana/grafana:develop
~~~

TODO Map the database

## Collect metrics
Now we need to collect that precious metrics. Easiest to do it is run sitespeed in your crontab. Edit your crontab:

~~~
crontab -e
~~~

And add something like this (make sure to change the URL and the host):

~~~
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
0,15,30,45 * * * * docker run --privileged --rm -v /sitespeed.io:/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -u http://mysite.com -b firefox -n 5 --connection cable -r /tmp/ --graphiteHost YOUR_GRAPHITE_HOST >> /tmp/sitespeed-run.txt 2>&1
~~~

You can of course fetch URL:s from a file and store the output if you want.

### Collect from multiple locations
It works perfectly to collect data from different servers/locations and send the data to the same Graphite server. What you need to do is give the keys in Graphite different names so that the don't collide. You do that by setting the *graphiteNamespace* when you run sitespeed and you have unique namespaces.

~~~
sitespeed.io -u http://mysite.com -b firefox --graphiteHost YOUR_GRAPHITE_HOST --graphiteNamespace sitespeed.newyork
~~~

## Setup your dashboards
To get up and running fast we have a [zip file]() with example JSON:s that you can use to. Remember though that you need to change the keys in to match your keys so you can see values.


# Example: Digital Ocean

In this example we will use Digital Ocean, just because they are super fast. Today they have data centers in San Francisco, New York, London, Amsterdam and Singapore, so you can choose to deploy on one of them or all of them.

If you are gonna test your site many times, you need to have the $20 instance, we have seen that the Chrome process eats a lot of the CPU.

1. Create a new droplet, choose the one with *2 GB / 2 CPUs 40 GB SSD Disk* and the region you want.
2. Click on the *Application* tab and choose *Docker on 14.04*
3. Remember to add the **SSH keys** for your user. Follow the [tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) of how to create your SSH keys.
4. Start your droplet.
5. When it is up and running, log into your server *ssh root@YOUR_IP*
6. Pull the Docker images needed:
*docker pull sitespeedio/sitespeeed.io* ,
*docker pull sitespeedio/graphite* and *docker pull grafana/grafana*
7. Start Grafana & Graphite:

~~~

~~~
8. **crontab -e** (choose nano)

~~~crontab

~~~
Make sure to edit your YOUR_IP
