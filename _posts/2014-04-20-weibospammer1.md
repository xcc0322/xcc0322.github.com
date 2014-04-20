---
layout: post
title: "WeiboSpammer-Chapter1 Scrapy"
description: ""
category: cs
tags: [Weibo Spammer, CS course]
---
{% include JB/setup %}
![Scarpy](http://doc.scrapy.org/en/latest/_images/scrapy_architecture.png){: style="display: block;margin-left: auto;margin-right:auto;width:100%"}

[Scrapy](http://scrapy.org/) is a popular crawler framework in Python. Recently I used it to build a weibo crawler to crawl some data for my WeiboSpammer project on Data Mining course.

I install the framework with Windows8 and Python2.7. Here are the details:

####1.Read [Scrapy at a glance](http://doc.scrapy.org/en/latest/intro/overview.html) to gain an overview.

####2.Install Scrapy.

* Install OpenSSL(install VC++2008 redistributables first)
* Install pip
* Install lxml(I fail to do this with pip but make it with easy_install)
* Install Scrapy

Then I start playing with the framework.

####3.Start with the tutorial

	>scrapy startproject tutorial

The command line tells me zope.install in need!

	>easy_install zope.install

There appears another error. Luckily someone else met it before and asked  on [stackflow](http://stackoverflow.com/questions/22800768/getting-error-dll-load-failed-the-operating-system-cannot-run-1-python-2-7).
I copied the ssleay.dll and libeay32.dll from openssl directory to system32. Restart. Tutorial project built!

I followed the instructions to write the classes. However I encountered another problem "No module named win32api". The tutorial said: You need to install pywin32 because of this Twisted bug. So I installed pywin32 from [here](http://sourceforge.net/projects/pywin32/files/pywin32/Build%20218/).
It works! The first crawler crawls two sites successfully.

####4.Customize
After this we have gain a basic overview of Scarpy. Now we can turn to our own project and learn further skills while solving our problems.