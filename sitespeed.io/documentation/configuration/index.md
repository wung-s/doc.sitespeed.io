---
layout: default
title: Browsers - Documentation - sitespeed.io
description: How to configure sitespeed.io
keywords: configuration, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: https://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Configuration for sitespeed.io.
---
[Documentation](/sitespeed.io/documentation/) / Configuration

# Configuration
{:.no_toc}

* Lets place the TOC here
{:toc}

# Configuration
Sitespeed.io is highly configurable, let's check it out!

## The options
You have the following options running sitespeed.io (we will add more as we are coming to the stable 4.0 release):

~~~help
bin/sitespeed.js [options] <url>/<file>

Browser
  --browsertime.browser, -b, --browser          Choose which Browser to use when you test [firefox|chrome]      [choices: "chrome", "firefox"] [default: "chrome"]
  --browsertime.iterations, -n                  How many times you want to test each page                                                             [default: 3]
  --browsertime.delay                           Delay between runs, in milliseconds
  --browsertime.connection, -c, --connectivity  Limit the connectivity speed by simulating connection types
                                                                                      [choices: "mobile3g", "mobile3gfast", "cable", "native"] [default: "native"]
  --browsertime.seleniumServer                  Configure the path to the Selenium server when fetching timings using browsers. If not configured the supplied
                                                NodeJS/Selenium version is used.
  --browsertime.viewPort                        The view port, the page viewport size WidthxHeight like 400x300                              [default: "1366x708"]
  --browsertime.userAgent                       The full User Agent string, defaults to the user agent used by the browsertime.browser option.
  --browsertime.preScript, --preScript          Task(s) to run before you test your URL (use it for login etc). Note that --preScript can be passed multiple
                                                times.
  --browsertime.postScript, --postScript        Path to JS file for any postTasks that need to be executed.

Crawler
  --crawler.depth, -d      How deep to crawl (1=only one page, 2=include links from first page, etc.)
  --crawler.maxPages, -m   The max number of pages to test. Default is no limit.
  --crawler.skip, -s       Do not crawl pages that contains this value in the URL path. NOT YET IMPLEMENTED
  --crawler.containInPath  Only crawl URLs that contains this value in the URL path. NOT YET IMPLEMENTED

Graphite
  --graphite.host                The Graphite host used to store captured metrics.
  --graphite.port                The Graphite port used to store captured metrics.                                                                 [default: 2003]
  --graphite.namespace           The namespace key added to all captured metrics.                                                        [default: "sitespeed_io"]
  --graphite.includeQueryParams  Whether to include query paramaters from the URL in the Graphite keys or not                           [boolean] [default: false]
  --graphite.data                Path to a JSON file to customize which metrics to send. NOT YET IMPLEMENTED

WebPageTest
  --webpagetest.host  The domain of your WebPageTest instance.                                                                    [default: "www.webpagetest.org"]
  --webpagetest.key   The API key for you WebPageTest instance.

Slack
  --slack.hookUrl   WebHook url for the Slack team (check https://<your team>.slack.com/apps/manage/custom-integrations).
  --slack.userName  User name to use when posting status to Slack.                                                                       [default: "Sitespeed.io"]

HTML
  --html.showWaterfallSummary  Set to true to show waterfalls on summary HTML report                                                    [boolean] [default: false]

Options:
  --version, -V   Show version number                                                                                                                    [boolean]
  --debug         Debug mode logs all internal messages to the console.                                                                 [boolean] [default: false]
  --verbose, -v   Verbose mode prints progress messages to the console. Enter up to three times (-vvv) to increase the level of detail.                    [count]
  --mobile        Access pages as mobile a mobile device. Set UA and width/height. For Chrome it will use device Apple iPhone 6.        [boolean] [default: false]
  --outputFolder  The folder name where the result will be stored. By default the name is generated using current_date/domain/filename
                                                                                                                                     [default: "sitespeed-result"]
  --firstParty    A regex running against each request and categorize it as first vs third party URL. (ex: ".*sitespeed.*")
  --utc           Use Coordinated Universal Time for timestamps                                                                         [boolean] [default: false]
  --config        Path to JSON config file
  --help, -h      Show help                                                                                                                              [boolean]

Read the docs at https://www.sitespeed.io/sitespeed.io/documentation/
~~~


## The basics
If you installed with the global option (-g), run the command *sitespeed.io* else run the script *bin/sitespeed.js*.  In the examples we will use the installed version.

You can analyze a site either by crawling or feed sitespeed.io with the URL:s you want to analyze.
