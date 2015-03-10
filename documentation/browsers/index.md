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
Firefox, Chrome, Internet Explorer (Windows only), Safari (Mac only, you need Safari 8).


# Fetching browser timings
You can fetch timings (Navigation Timing and User Timings) using real browsers using the **-b** flag. We use
    [Browsertime](https://github.com/tobli/browsertime) to collect the data. By a default
    installation you can fetch timings using Chrome and Firefox (as long as you have them installed on your machine).

There's also prepared support for Safari and Internet Explorer (Windows only). To test them out,you need to run your start your own [Selenium server](http://www.seleniumhq.org/download/). By default
    sitespeed.io will try to reach it at **http://localhost:4444/wd/hub**.

To get Safari to work, you need to install the [SafariDriver extension](https://github.com/SeleniumHQ/selenium/tree/master/javascript/safari-driver) in the browser. We haven't tested this yet, please help us out and update the doc on how to do it.

To get Internet Explorer to work, follow the [instructions](https://code.google.com/p/selenium/wiki/InternetExplorerDriver#Required_Configuration).

There's a problem using sitespeed.io/BrowserTime on Windows, where the BrowserMobProxy isn't closed/killed between runs, you need to kill it yourself right now.
{: .note .note-warning}

## Timing metrics
All the metrics are collected using an empty cache

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
