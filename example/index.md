---
layout: default
title: Example of a sitespeed run.
description: Here is examples of what the result looks like, when you run sitesspeed.io.
author: Peter Hedenskog
keywords: sitespeed.io, examples, results, wpo, web performance optimization
nav: examples
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
---

# Examples

Crawling, analyzing and fetching timings:

~~~ bash
sitespeed.io -u http://www.cybercom.com -b chrome
~~~
Gives the following [report](http://examples.sitespeed.io/3.0/2014-12-15-22-16-30).

Crawling, analyzing and fetching timings using WebPageTest and Google Page Speed Insight:

~~~ bash
sitespeed.io -u http://www.cybercom.com -b chrome --wptHost YOUR_HOST --gpsiKey YOUR_KEY
~~~

Gives the following [report](http://examples.sitespeed.io/3.0/2014-12-19-12-18-17/).
