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


## Background
We have put a lot of love into making it easy to create your own performance dashboard. To get it up and running you need [Docker](https://www.docker.com/). That's it :)

The base is the Docker images:

  * To collect metrics you use one of the three images: [sitespeed.io with Chrome, Firefox & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io/),  [sitespeed.io with Chrome, & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io-chrome/) or  [sitespeed.io with Firefox & Xvfb](https://registry.hub.docker.com/u/sitespeedio/sitespeed.io-firefox/).
  * [Store the metrics using Graphite ](https://registry.hub.docker.com/u/sitespeedio/graphite/). If you have an Graphite server up and running already you can use that one, just make sure to configure *storage-schemas* and *storage-aggregations*.
  * [Graph the metrics using Grafana](https://registry.hub.docker.com/u/grafana/grafana/).

You can run these images on your own machine(s) or in the cloud. You only need Docker. But what will you get? We have setup an example site so you can try it out yourself. We are proud to present
[dashboard.sitespeed.io](http://dashboard.sitespeed.io:3000/). Login using *viewer/viewer*.

## Metrics and what you can graph

There's a lot of metrics collected, lets check what kind of views of the data you can create:

* [Show how each and every page is built (number of requests, request types etc), how the page score for rules and  timing metrics (Navigation Timing & User Timing)](http://dashboard.sitespeed.io:3000/dashboard/db/metric-for-one-page-american-airlines-home-page). Use this to keep track of important pages on your site.

* [Summary for a whole site]((http://dashboard.sitespeed.io:3000/dashboard/db/summary-of-a-site-america-airlines)), showing how your site is built, rules & timings. Use this to keep track of your site or your most imported pages over time.

* [Compare multiple sites](http://dashboard.sitespeed.io:3000/dashboard/db/compare-multiple-sites) and see how they are doing. Compare timings and how the pages are built.

* [Fetch metrics using WebPageTest](http://dashboard.sitespeed.io:3000/dashboard/db/using-webpagetest), will collect things like SpeedIndex, firstPaint, TTFB, render, visualComplete, domContentLoadedEventEnd, loadTime, page size, image size, number of requests for first and repeat view.

* [Timings per domain](http://dashboard.sitespeed.io:3000/dashboard/db/load-timings-per-domain) - a way to check how your third party content.

* [Timings per asset](http://dashboard.sitespeed.io:3000/dashboard/db/load-timings-per-asset) - here you can graph every request on a page and the timings: *blocked*, *dns*, *connect*, *ssl*, *send*, *wait*, *receive* and *total* time. It will generate a lot of data but is extremely good to find slow loading assets from a 3rd party.


## Setup the containers

It is easy to setup the containers. The only thing you need to do is setup directories where you store the data and start the containers.

### Graphite
First we want to have have Graphite to store the metrics. You want to store the data outside of your containers, so create an directory where you store the data. In this example we put it in */data/graphite/storage/whisper*

~~~
sudo mkdir -p /data/graphite/storage/whisper
~~~

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

Your Graphite instance will be behind Basic Auth (*guest/guest*), if your server is public you should change that by generating your own .httpwd file. You can do that with [apache2-utils](http://httpd.apache.org/docs/2.2/programs/htpasswd.html). You run it like this:

~~~
sudo apt-get install apache2-utils
sudo htpasswd -c .htpasswd YOUR_USERNAME
~~~

And add this line when you start the Graphite docker image:

~~~
-v /your/path/.htpasswd:/etc/nginx/.htpasswd \
~~~

The full startup would then look like this:

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


### Grafana
Before you start Grafana you want to make sure that the dashboard data is stored on disk. Create a directory that will hold the Grafana database:

~~~
sudo mkdir -p /data/grafana/data
~~~

And then start Grafana, map the directory, add a new admin user & password.

~~~
sudo docker run -d -p 3000:3000 \
-v /data/grafana/data:/usr/share/grafana/data \
-e "GF_SECURITY_ADMIN_USER=myuser" \
-e "GF_SECURITY_ADMIN_PASSWORD=MY_SUPER_STRONG_PASSWORD" \
--name grafana \
--restart="always" \
grafana/grafana
~~~

The next step is to access your Grafana instance and configure to use your Graphite instance as backend. Choose *Grafana admin* > *Data Sources* > *Add new*. And then make sure to set it as default and enable Basic Auth.

![Configure Grafana to use Graphite](configure-grafana.jpg)
{: .img-thumbnail}


### Collect metrics
Now we need to collect that precious metrics. Do it by the old crontab. But first create a data dir where you can put the input/output files for sitespeed.io:

~~~
sudo mkdir /sitespeed.io
~~~

Then edit your crontab:

~~~
sudo crontab -e
~~~

And add something like this (make sure to change the URL and the host). In this example we run every 15 minutes, but you can of course change it:

~~~
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
0,15,30,45 * * * * docker run --privileged --rm -v /sitespeed.io:/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -u http://mysite.com -b firefox -n 5 --connection cable -r /tmp/ --graphiteHost YOUR_GRAPHITE_HOST --seleniumServer http://127.0.0.1:4444/wd/hub >> /tmp/sitespeed-run.txt 2>&1
~~~

You can of course fetch URL:s from a file and store the output if you want. To do that add a file in you */sitespeed.io/* directory containing all the URL:s and run it like this:

~~~
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
0,15,30,45 * * * * docker run --privileged --rm -v /sitespeed.io:/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -f urls.txt -b firefox -n 11 --connection cable -r /tmp/ --graphiteHost YOUR_GRAPHITE_HOST --seleniumServer http://127.0.0.1:4444/wd/hub >> /tmp/sitespeed-run.txt 2>&1
~~~

Note: You need to configure the selenium server to use (there are bugs running it straight with NodeJS and Linux). You do that by adding --**seleniumServer http://127.0.0.1:4444/wd/hub** to your run. Using the selenium server will make your runs more stable.
{: .note .note-warning}

#### Collect from multiple locations
It works perfectly to collect data from different servers/locations and send the data to the same Graphite server. What you need to do is give the keys in Graphite different names so that the don't collide. You do that by setting the *graphiteNamespace* when you run sitespeed and you have unique namespaces.

~~~
sitespeed.io -u http://mysite.com -b firefox --graphiteHost YOUR_GRAPHITE_HOST --graphiteNamespace sitespeed.io.newyork
~~~

## Setup your dashboards
To get up and running fast we have a [zip file](dashboards.zip) with example JSON:s that you can use to. Remember though that you need to change the keys in to match your keys so you can see values.

If you need help, checkout the [Grafana documentation](http://docs.grafana.org/).

## Limit the amount of data in Graphite
If you test many pages you will have a lot of data stored in Graphite, make sure you set it up correctly for what you need.

### Configure sitespeed.io what metrics to send

You can choose to send the following metrics to Graphite:

* *summary* - the summary of a whole sites, it's the same data as shown on the summary HTML page
* *rules* - how each and every tested page match against the web performance best practice rules
* *pagemetrics* - how each and every page is built, like the number of javascripts, css etc
* *timings* - the timings for every page fetched using the Navigation Timing API and User Timings
* *requesttimings* - send the timings data for each and every request: *blocked*, *dns*, *connect*, *ssl*, *send*, *wait*, *receive* and *total* time. This will generate a lot of data.

By default all timings are sent. If you want to change that, remove the ones of the metrics you don't need:

~~~
--graphiteData summary,rules,pagemetrics,timings,requesttimings
~~~

### Configure Graphite what data to keep

By default the metrics are stored for 60 days (except request timings they are only stored for 7 days by default) and you can change that. First [read]((https://github.com/etsy/statsd/blob/master/docs/graphite.md)) Etsy:s nice writeup on how you configure Graphite. Create your own [storage-schemas.conf](https://github.com/sitespeedio/docker-graphite-statsd/blob/master/conf/graphite/storage-schemas.conf) file and feed it to the image on startup like this:

~~~
-v /path/to/storage-schemas.conf:/opt/graphite/conf/storage-schemas.conf
~~~

You can also configure how data is aggregated over time. Check out the default configuration [storage-aggregation.conf](https://github.com/sitespeedio/docker-graphite-statsd/blob/master/conf/graphite/storage-aggregation.conf) and reread Etsys nice writeup :)

## Add events/annotations to the graphs

Graphite comes with an [event API](http://obfuscurity.com/2014/01/Graphite-Tip-A-Better-Way-to-Store-Events) so you can mark specfic events like releases. You can simply do that with a curl!

~~~
curl -u LOGIN:PASSWORD -X POST "http://HOSTNAME:8080/events/" -d '{"what": "Deploy", "tags": "production deploy example", "data": "deploy of master branch, version 1.0.0"}'
~~~

Change the LOGIN and PASSWORD to the Basic Auth you are using for Graphite and the HOSTNAME to your host. Then for each dashboard choose *Annotations* and the one you use by the tag(s).


# Example setup: Digital Ocean

In this example we will use [Digital Ocean](https://www.digitalocean.com/), because they are super fast. Today they have data centers in San Francisco, New York, London, Amsterdam and Singapore. You can choose to deploy on one of them or all of them.

When we've been testing, we have seen that you can Firefox or Chrome on a $5 instance (remember to setup the swap space!). In this example we will use a $20 instance and put everything on that.

* Create a new droplet, choose the one with *2 GB / 2 CPUs 40 GB SSD Disk* and the region you want.
Click on the *Application* tab and choose *Docker on 14.04*
* Remember to add the **SSH keys** for your user. Follow the [tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets) of how to create your SSH keys.
* Start your droplet.
* When it is up and running, log into your server *ssh root@YOUR_IP*
* Setup the server following Digital Oceans [Initial Server Setup Guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04) to make your server a little more secure.
* [Add swap space](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04) and avoid out pf memory errors.
* Pull the Docker images:
*sudo docker pull sitespeedio/sitespeeed.io* ,
*sudo docker pull sitespeedio/graphite* and *sudo docker pull grafana/grafana*
* Create the directories:

~~~
sudo mkdir -p /data/graphite/storage/whisper
sudo mkdir -p /data/grafana/data
sudo mkdir /sitespeed.io
~~~
* Start Grafana & Graphite (first create your own .htpasswd file and change the user and admin user password):

~~~
sudo docker run -d \
  --name graphite \
  -p 8080:80 \
  -p 2003:2003 \
  --restart="always" \
  -v /data/graphite/storage/whisper:/opt/graphite/storage/whisper \
  -v /your/path/.htpasswd:/etc/nginx/.htpasswd \
  sitespeedio/graphite

sudo docker run -d -p 3000:3000 \
-v /data/grafana/data:/usr/share/grafana/data \
-e "GF_SECURITY_ADMIN_USER=myuser" \
-e "GF_SECURITY_ADMIN_PASSWORD=MY_SUPER_STRONG_PASSWORD" \
--name grafana \
--restart="always" \
grafana/grafana
~~~

* Create a file with the URL:s you want to test by *sudo nano /sitespeed.io/urls.txt*:

~~~
http://www.myfirsturl.com
http://www.myfirsturl.com/1/
http://www.myfirsturl.com/2/
http://www.myfirsturl.com/3/
~~~

* **sudo crontab -e** (choose nano and make sure to edit your YOUR_GRAPHITE_HOST to the IP of your server).

~~~
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
0,15,30,45 * * * * docker run --privileged --rm -v /sitespeed.io:/sitespeed.io sitespeedio/sitespeed.io sitespeed.io -f urls.txt -b firefox -n 11 --connection cable -r /tmp/ --graphiteHost YOUR_GRAPHITE_HOST --seleniumServer http://127.0.0.1:4444/wd/hub >> /tmp/sitespeed-run.txt 2>&1
~~~


* The next step is to log into your Grafana instance and configure Graphite as your backend. Then you can start creating your own dashboards.
