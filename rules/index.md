---
layout: default
title: The performace best practices rules used by Sitespeed.io
description: Here are the list of performance best practices rules used by sitespeed.io when analyzing your website. It will check for SPOF, synchronously loaded javascripts inside head and a lot of more things.
author: Peter Hedenskog
keywords: sitespeed.io, rules, wpo, fast, speed, web performance optimization, best practices
nav: rules

---
# The rules
{:.no_toc}

Sitespeed.io uses performance best practices rules to decide if a page is optimized for performance. The idea of rules for performance was started by Steve Souders
[14 rules for faster loading websites](http://stevesouders.com/hpws/rules.php) in 2007. Those rules and a couple of more is implemented in YSlow. Sitespeed.io uses these rules and approx. 20 more (it has happened things since 2007). You can see all rules used [here](#allrules). There are two set of rules; desktop & mobile. Desktop is the default one and described on this page. The mobile rules are the same rules, but they have different weight.
    You can see the exact implementation [here](https://github.com/soulgalore/yslow/blob/master/src/common/rulesets/ruleset_sitespeed.js).

## Don't break the rules!
{:.no_toc}

Sitespeed.io is very demanding on your site, it will test/analyze and punish hard if you have broken the most important rules.

Sitespeed.io uses about 20 of the basic rules from YSlow (read about them
        [here](http://yslow.org#web_performance_best_practices_and_rules) and [here](https://github.com/marcelduran/yslow/wiki/Ruleset-Matrix) and then there are the sitespeed.io specific rules.

## The rules
{:.no_toc}

* Lets place the TOC here
{:toc}  

### Critical Rendering Path
Every request fetched inside of HEAD, will postpone the rendering of the page! Do not load javascript synchronously inside of head, load files from the same domain as the main document (to avoid DNS lookups) and inline CSS for a really fast rendering path. The scoring system for this rule, will give you minus score for synchronously loaded javascript inside of head, CSS files requested inside of head and minus for every DNS lookup inside of head. If you want to have a really fast first paint (and you always want that), make sure to score high on this rule.


You can read more about the critical rendering path in [this](http://calendar.perfplanet.com/2012/deciphering-the-critical-rendering-path/) article by Ilya Grigorik and [this](http://www.phpied.com/css-and-the-critical-path/) post by Stoyan Stefanov.
{: .note .note-info}


### Never scale images in HTML
Images should always be sent with the correct size else the browser will download an image that is larger than necessary. This is more important today with responsive web design, meaning you want to avoid downloading non scaled images to a mobile phone or tablet. Note: This rule doesn't check images with size 0 (images in carousels etc), so they will be missed by the rule.


Set the viewport size when you analyze a page and you will get the real image size & the image screen size with this rule!
{: .note .note-info}

### Frontend single point of failure (SPOF)
A page can be stopped to be loaded in the browser, if a single script, css and in some cases a font couldn't be fetched or loading slow (the white screen of death!). You really want to avoid that. Never load 3rd party components inside of head!  One important note, right now this rule treats domain and subdomains as ok, that match the document domain, all other domains is treated as a SPOF. The score is calculated like this: Synchronously loaded javascripts inside of head, hurts you the most & then CSS files inside of head hurts a little less. You can also look for Font Face inside of CSS files and  inline font face files, but they are considered minor and not turned on by default. One rule SPOF rule missing is the IE specific feature, that a font face will be SPOF if a script is requested before the font face file.



Read more about SPOF in [this](http://www.stevesouders.com/blog/2010/06/01/frontend-spof/") nice blog post by Steve Souders.
{: .note .note-info}


### Low number of total requests
Avoid have too many requests on your page. The more requests, the slower the page will be. This is extremely important on mobile devices, keep the requests as few as possible. But remember that using SPDY and the forthcoming HTTP 2.0, requests to the same domain will be done at the same time, meaning the total number of requests will be less important.

### Do not load specific CSS file for print
Loading a specific stylesheet for print, can block rendering in your browser (depending on version) and will for almost all browsers, block the onload event to fire (even though the print stylesheet isn't even used). In the old times, it was ok to have own print styles, but nowadays you can keep them in the same CSS file.


You can read more about it [here](http://www.phpied.com/5-years-later-print-css-still-sucks/).
{: .note .note-info}

### Load CSS in HEAD from document domain
CSS files inside of the HEAD tag, should be loaded from the same domain as the main document, in order to avoid DNS lookups. This is extra important for mobile. This rule exist in YSLow don't work on PhantomJS. This modified version do.

### Never load JS synchronously in HEAD
Javascript files should never be loaded synchronously inside the HEAD tag, because it will block the rendering of the page. This rule exist in YSLow don't work on PhantomJS. This modified version do.

### Avoid use of web fonts
Avoid use of web fonts because they will decrease the performance of the page. Web fonts are faster today then they ever has been, but still they will decrease performance.


Read [more](http://www.stevesouders.com/blog/2009/10/13/font-face-and-performance/) about avoiding web fonts.
{: .note .note-info}

### Have expire headers for static components
This is a modified version of YSlow expire rule, it will look for assets without any expire headers, meaning all assets without any expire headers will be reported.

### Have expires headers equals or longer than one year
Having really long cache headers are beneficial for caching.

### Avoid DNS lookups when the page has few request
If you have few requests on a page, they should all be on the same domain to avoid DNS lookups, because the lookups will cost much. This rule only kicks in if you have fewer request than 10 on the page.

### Do not load stylesheet files when the page has few request
When a page has few requests (or actually maybe always if you don't have a massive amount of CSS), it is better to inline the CSS, to make the page to start render as early as possible.

### Have a reasonable percentage of textual content compared to the rest of the page
Make sure you don't have too much styling etc that hides the text you want to deliver.


### Load third party JS asynchronously
Always load third party javascript asynchronously. Third parties that will be checked are Twitter, Facebook, Google (api, analytics, ajax), LinkedIn, Disqus, Pinterest & JQuery.


Read more about asynchronously third party scripts [here](http://www.phpied.com/3PO#async). This rule is borrowed from Stoyan Stefanov :)
{: .note .note-info}
