---
layout: default
title: Sitespeed.io - Release notes 3.2
description: The new sitespeed.io has support for driving multiple WebPageTest locations/browsers/connectivity and choose to collect metrics using PhantomJS or SlimerJS.
author: Peter Hedenskog
keywords: sitespeed.io, release, release-notes, 3.2
nav:
image:  http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: The new sitespeed.io has support for driving multiple WebPageTest locations/browsers/connectivity and choose to collect metrics using PhantomJS or SlimerJS.
---

# Sitespeed.io 3.2
We've been releasing many small 3.1.x releases and now it's time for 3.2. We've doing two important changes here:
 * We have changed the way of fetching multiple sites. In the new version you can configure multiple sites by adding the parameter --sites one time for each site. The main reason is that it simpler and also makes it easier to run in our Docker container.
 * We have decreased the default size of the memory for the Java crawler. The old default (1024 mb) was good for crawling thousend of URL:s so if you are doing that today, add the parameter *--memory 1024* when you run the script. 1024 works bad on small machines so 256 is the new default.

The new 3.2 helps us getting ready for the next cool feature, lets stay tuned the coming month.

 Intresting new stuff in the 3.1.x releases:
 * cleanup of the perf budget handling. the script will return an error code if the budget fails and will log failing budget metrics and an HTML page showing the budget report.
 * Upgraded BrowserTime that works as it should on Windows
 * Send domain timings to Graphite and timings per request.
 * No robot removed from the HTML pages
 * Many small bug fixes, checkout the [complete list](https://github.com/sitespeedio/sitespeed.io/blob/master/CHANGELOG.md)
