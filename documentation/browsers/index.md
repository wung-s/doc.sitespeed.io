---
layout: default
title: Browsers - Documentation - sitespeed.io
description: How to get browser timings using sitespeed.io
keywords: configuration, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Browser timings for the sitespeed.io.
---
[Documentation](/documentation/) / Browsers

# Browsers
{:.no_toc}

* Lets place the TOC here
{:toc}

You can fetch timings and execute your own Javascript. The following browsers are supported:
Firefox, Chrome, Internet Explorer and Safari.

## Firefox
Firefox will work out of the box, as long as you have Firefox installed on your machine.

## Chrome
You need to install Chrome and the [Chromedriver](https://sites.google.com/a/chromium.org/chromedriver/) to be able to collect metrics from Chrome.

## Internet Explorer
Windows only. To get Internet Explorer to work, follow the [instructions](https://code.google.com/p/selenium/wiki/InternetExplorerDriver#Required_Configuration).

## Safari
You need Safari 8 to get timiings from your browser (Mac only). To get it to work, you need to install the [SafariDriver extension - SafariDriver.safariextz](http://selenium-release.storage.googleapis.com/index.html?path=2.45/) in your browser. With the current version no HAR-file is created.

# Fetching timings
You can fetch timings ([Navigation Timing](http://www.w3.org/TR/navigation-timing/) and [User Timings](http://www.w3.org/TR/user-timing/)) using using the **-b** flag. We use [Browsertime](https://github.com/tobli/browsertime) to collect the data.

~~~ bash
$ sitespeed.io -u http://yoursite.com  -b firefox
~~~

What we do is run a couple of [Javascripts](https://github.com/tobli/browsertime/tree/master/lib/scripts) that collects metrics from the browser. The session ends when the *window.performance.timing.loadEventEnd* happens (but you can configure that yourself). 

## Simulate the connection speed

## Choose when to end your test

## Fetch your own metrics from the browser


## Collected timing metrics
All the metrics are collected using an empty cache.

* *backEndTime* - The time it takes for the network and the server to generate and start sending the HTML. Definition: responseStart - navigationStart
* *domContentLoadedTime* - The time the browser takes to parse the document and execute deferred and parser-inserted scripts including the network time from the users location to your server. Definition: domContentLoadedEventStart - navigationStart
* *domInteractiveTime* - The time the browser takes to parse the document, including the network time from the users location to your server. Definition: domInteractive - navigationStart
* *domainLookupTime* - The time it takes to do the DNS lookup. Definition: domainLookupEnd - domainLookupStart
* *frontEndTime* - The time it takes for the browser to parse and create the page. Definition: loadEventStart - responseEnd
* *pageDownloadTime* - How long time does it take to download the page (the HTML). Definition: responseEnd - responseStart
* *pageLoadTime* - The time it takes for page to load, from initiation of the page view (e.g., click on a page link) to load completion in the browser. Important: this is only relevant to some pages, depending on how you page is built. Definition: loadEventStart - navigationStart
* *redirectionTime* - Time spent on redirects. Definition: fetchStart - navigationStart
* *serverConnectionTime* - How long time it takes to connect to the server. Definition: connectEnd - connectStart
* *firstPaint* - This is when the first paint happens on the screen. If the browser support this metric, we use that. Else we use the time of the last non-async script or css from the head. You can easily verify if the first paint metrics is valid for you, by record a video using WebPageTest, and then check exactly when the first paint happens and compare that with the timing from the browser.
* [RUM-SpeedIndex](https://github.com/WPO-Foundation/RUM-SpeedIndex) - created by Pat Meenan
and calculate SpeedIndex measurements using Resource Timings. It iss not as perfect as Speed Index in WPT but a good start.
