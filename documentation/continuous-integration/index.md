---
layout: default
title: Continous integration - Documentation - sitespeed.io
description: How to configure sitespeed.io
keywords: configuration, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: http://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Configuration for the sitespeed.io.
---
[Documentation](/documentation/) / Continous Intergration

# Continuous integration
{:.no_toc}

* Lets place the TOC here
{:toc}

Use sitespeed.io to keep track of what is happening with your site and make sure you know that you don't break the performance best practice rules **before** your changes are released.
Your build can also be broken if your page timings (choose whatever timing you need from the Navigation Timing API or your own custom timing) break your limits. And of course, if you use WebPageTest or Google Page Speed Insights, you can use the metrics from them to break your build.

Sitespeed.io outputs [JUnit XML](http://help.catchsoftware.com/display/ET/JUnit+Format) and [TAP](http://testanything.org/) that test your metrics against your configured limits.


## Generating JUnit XML or TAP
Sitespeed can output **junit.xml** that works perfect with your continuous integration tool. Jenkins, Bamboo and others support it out of the box.

~~~bash
$ sitespeed.io -u http://www.sitespeed.io --junit
~~~

Or if you rather prefer TAP:

~~~bash
$ sitespeed.io -u http://www.sitespeed.io --tap
~~~

## Configure when to break a test
By default these are the values that are tested:

~~~
{
  "rules": {
    "default": 90
  },
  "timings": {
    "serverResponseTime": 300,
    "domContentLoadedTime": 700,
    "speedIndex": 1000
  },
  "wpt": {
    "SpeedIndex": 1000
  },
  "gpsi": {
    "score": 90
  }
}
~~~

This means that all rules must be 90 or better,serverResponseTime 300 ms or better,
domContentLoadedTime to be faster than 700 ms. If you use WebPageTest, the speed index need to be under 1000. And finally, if you use Goggle Page Speed Insights, the score must be 90 or better.


You can configure to point out a JSON file containing all the values:

~~~bash
$ sitespeed.io -u http://www.sitespeed.io --tap --budget myBudget.json
~~~

## Skip rule tests
By default a all rules is tested against the default number you specify, meaning all rules needs to be 90 or better. Sometimes you have some rules you don't care about and can skip. You do that by feeding the rule names to the script. You find all the names [here](https://github.com/sitespeedio/yslow/blob/master/src/common/rulesets/ruleset_sitespeed.js).

~~~bash
$ sitespeed.io -u http://www.sitespeed.io --skipTest ycdn,textcontent --tap
~~~

## Jenkins
You have can use sitespeed.io in [Jenkins](http://jenkins-ci.org/") either by running as a CLI (*Execute shell*) or by using the [sitespeed.io plugin](https://github.com/sitespeedio/jenkins.sitespeed.io).

You want the browsers to run headless, use the [Xvfb plugin](https://wiki.jenkins-ci.org/display/JENKINS/Xvfb+Plugin) to make it happen!


### Running as CLI

* Choose **New Item** and create a new freestyle project.
* Choose **Add build step** in the Build part and **Execute shell** you will have a box where you add your sitespeed.io CLI magic. Remember that the Jenkins user needs to have NodeJS in the path. It can look like this (sending the data to a local Graphite instance):

~~~
sitespeed.io -u http://www.cybercom.com --graphiteHost localhost --graphiteNamespace cybercom-production -b chrome -n 11
~~~

* If you want to break your build, you can either generate JUnit XML and use the built in post task **Publish JUnit test result report**.
* In the execute shell form: *sitespeed.io -u http://stage.cybercom.com --resultBaseDir ${WORKSPACE}/${BUILD_NUMBER} --junit > junit.xml* And in the post task **Test report XMLs** add: *junit.xml*
* Using TAP, you need to install the [TAP plugin](https://wiki.jenkins-ci.org/display/JENKINS/TAP+Plugin).
* Run the execute shell like this *sitespeed.io -u http://stage.cybercom.com --resultBaseDir ${WORKSPACE}/${BUILD_NUMBER} --tap > sitespeed.tap*
* And choose the post task **Publish TAP Results** and in the Test Results box add: *sitespeed.tap*

### Jenkins plugin
The Jenkins plugin is not yet distributed within Jenkins, so you need to build and install it yourself. Follow [these](https://github.com/sitespeedio/jenkins.sitespeed.io#how-to-run-in-jenkins) instructions on how to do it. Remember that you need to have NodeJS in the path for the user running the plugin and make sure the user has the rights to execute the sitespeed.io executable.

The plugin focus on breaking your build if your budget doesnt't match the real world. You can choose to output the result as JUnit XML (the file is named **sitespeed.io-junit.xml*), TAP (**sitespeed.io-junit.tap**) or a budget file. If you run *budget* the script will return with a error return code, if your budget breaks. JUnit & TAP will always return ok, so then you need to setup the **Test report XMLs** or the **Publish TAP Results** task to break your build.


Add sitespeed.io in your build step:

![Add sitespeed.io as a build step](add-build-step-jenkins.png)
{: .img-thumbnail}

You can configure the plugin like this:

![Configure the plugin](jenkins-plugin-configuration.png)
{: .img-thumbnail}

And remember, if you output TAP, use the [TAP plugin](https://wiki.jenkins-ci.org/display/JENKINS/TAP+Plugin) or JUnit use the built in **Publish JUnit test result report**.


## Travis integration
Coming soon!


## Grunt plugin
There's a Grunt plugin [grunt-sitespeedio](https://github.com/sitespeedio/grunt-sitespeedio) where you can do all the things you usually do with sitespeed.io. Use it create HTML-reports, send metrics to Graphite or test your performance budget.
