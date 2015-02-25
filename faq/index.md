---
layout: default
title: Frequently asked questions for sitespeed.io
description: Questions and answers for sitespeed.io.
author: Peter Hedenskog
keywords: sitespeed.io, faq, questions, frequently, asked
nav: faq

---

# FAQ
{:.no_toc}

If you don't find the answer here or in the [documentation](/documentation/), please ping me on [Twitter](https://twitter.com/SiteSpeedio) or add an issue on [Github](https://github.com/sitespeedio/sitespeed.io/issues?state=open).

* Lets place the TOC here
{:toc}


## I don't get all values in Graphite?
Doesn't all values reach Graphite (are you missing values), then you should check your **carbon.conf** file. The configuration **MAX_CREATES_PER_MINUTE** needs to be set to high (we have seen that 50 is too low). To make sure it works, set it like this:
*MAX_CREATES_PER_MINUTE = inf*


## Firefox don't work on Linux
If you run on Linux without a graphical desktop, remember that you need to use [Xvfb](http://www.x.org/archive/current/doc/man/man1/Xvfb.1.xhtml) to get it to work.
If you don't, the error you will get is that Firefox can't install the profile. Something like this: *Failed to install profile; firefox terminated ...*

## Chrome/Chromedriver failing on Ubuntu?
Yep, when running many tests after each other (2000+), sometimes the Selenium NodeJS version fails
starting Chrome (issue reported [here](https://code.google.com/p/selenium/issues/detail?id=8261)). We are working on a workaround.

## I have a URL that can't be analyzed, what should I do?
First make sure you run the [latest version](https://www.npmjs.com/package/sitespeed.io) of sitespeed.io. If you do and you still get the error, please report it as an [issue](https://github.com/sitespeedio/sitespeed.io/issues/new).

## Are there any known bugs?

Checkout the [buglist](https://github.com/sitespeedio/sitespeed.io/issues?labels=bug&state=open) for latest updated list of defects in sitespeed.

There are one inherited bugs from PhantomJS/YSlow:

* The gzipped size of a component is not working so right now the unzipped size is showed everywhere. See original defect [here](http://code.google.com/p/phantomjs/issues/detail?id=156).

If you find other bugs, please report them [here](https://github.com/sitespeedio/sitespeed.io/issues).


## I get java.io.UnsupportedEncodingException: gzip, what does that mean?
The crawler used in sitespeed.io doesn't fetch content using GZIP (sending header that accepts gzip), however some sites sends content gzipped to all clients and then is when you get this exception.


## My HAR files don't look right when I test a HTTPS site?
There's a known problem collecting HAR data for HTTPS. Checkout [BrowserMobProxy](https://github.com/lightbody/browsermob-proxy) of how you can handle it.

## I get *Error: no display specified*
If you run on Linux and want to emulate a display (a.k.a running a browser without a screen) then you can do two things. If you are using Jenkins, then  use the [Xvfb plugin](https://wiki.jenkins-ci.org/display/JENKINS/Xvfb+Plugin).
  Else you need to:

* Install [XVFB](http://www.x.org/releases/current/doc/man/man1/Xvfb.1.xhtml).
* Setup a start script, something like [this](https://gist.github.com/jterrace/2911875).
* Export the display, make sure to have the same number as in the examle in the Gist: *export DISPLAY=:99.0*
* Start Xvfb: *sh -e /etc/init.d/xvfb start*

You can check out the projects Travis configuration [file](https://github.com/sitespeedio/sitespeed.io/blob/master/.travis.yml) for an example.

## I get java.net.SocketTimeoutException: Read timed out. Why?
The crawler has a configurable timeout time (you find it in *dependencies/crawler.properties*).  The crawler crawls your site by first fetching the start page and then the rest of the pages using a thread pool (that you can configure in the same property file). Default is five threads, meaning that it will be maximum five concurrent threads accessing your site. If your site has problem with five (yes that has actually happen) concurrent users, the crawler can timeout. The solution is to increase the timeout time in the properties file.

## I want to add my own rules, how do I do that?
Check out the [documentation](/documentation/#add-your-own-rules).


## Is there a option to exclude a given directory?
Yes, kind of, checkout the [include/exclude section](/documentation/#includeexclude-urls-when-crawling). Use the option **-s** that will skip urls that contains the specified text in the path.

## I want to test only one page, how do I do that?
~~~ bash
$ sitespeed.io -u http://mysite.com -d 0
~~~

## Can I use sitespeed on Windows?
Yes we hope it will work! We don't have any continous integration setup for Windows, so it is hard for us to test that it really work.
