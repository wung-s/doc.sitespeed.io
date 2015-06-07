---
layout: default
title: Sitespeed.io - Release notes 3.6.0
description: Say hello to scripts and custom metrics for WebPageTest.
author: Peter Hedenskog
keywords: sitespeed.io, release, release-notes, 3.6.0
nav:
image:  http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Say hello to scripts and custom metrics for WebPageTest.
---

# Sitespeed.io 3.6.0
We have a couple of cool things in 3.6.0!

## WebPageTest
If you run WebPageTest through sitespeed.io. We now supports WebPageTest scripts (so you can login etc) and custom metrics (define your own javascript to fetch metrics). WebPageTest custom metrics are also sent to Graphite! Read more about how to run your [custom script](/documentation/webpagetest/#webpagetest-scripting) and to collect [custom metrics](/documentation/webpagetest/#custom-metrics).

## Browsertime with Firefox 38
We also have a new upgraded version of Browsertime that gives us a new Selenium that works with Firefox 38, has the possibility to run Chrome without sandbox mode (use the **btConfig** flag to configure specific Browsertime things) and two new connection speeds: mobile2g and mobile3gslow. The two speeds will be availible in sitespeed in a while.

## Send Google Analytics metrics to Graphite
We also have something really cool :) Inspired by Etsys [Google Analytics to Graphite](https://github.com/etsy/GoogleAnalyticsToGraphite) we added support for the Google API:s 3.0 and created a [NodeJS project](https://www.npmjs.com/package/gatographite) where you can send Google Analytics data to Graphite. You can configure what metrics to export so it is up to you what you want to do with it. We also made a [Docker container]( https://registry.hub.docker.com/u/sitespeedio/gatographite/) that makes it super easy to add on your Open Source Web Performance Dashboard.

You can checkout our version [http://dashboard.sitespeed.io/dashboard/db/google-analytics](http://dashboard.sitespeed.io/dashboard/db/google-analytics) that fetch Google Analytics metrics once every night.

## Updated Docker containers
All [Docker containers](https://registry.hub.docker.com/repos/sitespeedio/) now run the latest version of sitespeed.io, Firefox & Chrome. To be sure that your tests never brakes use the tagged version of each container.

## Reach out!
Hey we also want to reach out to the company/individual that downloaded our main sitespeed.io Docker container 20000 times. We would really love to hear how you use it, please tell us on [Twitter](https://twitter.com/sitespeedio)!
