---
layout: post
title: "WeiboSpammer-Chapter2 WeiboCrawler using Scrapy"
description: ""
category: cs
tags: [Weibo Spammer, CS course]
---
{% include JB/setup %}

After a bite of Scrapy, we are going to make it work with our target sites, weibo in this case.

According to the need of my project, I design a crawler with following jobs:

1. Scratch basic info(numbers of followings and fans, quality of most recent posts) as a signal judging the possibility of anomaly.

2. Move to the following and fans pages to crawl the uids of the _followed users(followings)_ and _fans_. Store the connections to build the link graph.

3. Crawl and add the profile pages to request list of the users we met in the second step. Repeat step1 and step2.

####Log in

Tricky is to log in and acquire the first page. I referred to the work [here](http://qinxuye.me/article/simulate-weibo-login-in-python/).

The machinism is a little complicated. We would have to observe the rules little by little with Firebug-like tools. Oberserve parameters in our posts and the content returned by weibo server.

After following the steps by the link above, I finally make it arrive at the first user profile page of mine. Making sure that this works, I start to build the spider.

####Customize Scrapy

The main tasks here are to make our own **Spider, Item and ItemPipeline** classes, as well as to modify some settings.

######Spider
In Spider class, I build the log-in, parsing and extracting data, and crawling further links procedures. Make every procedure a seperate class and assembly them in WeiboSpider class.

The concepts are simple. Here are some key points.

Remember the graph in Chapter 1. The output of Spider class should be Requests and Items to be dispatched to Downloader and ItemPipeline respectively. We either set some *start_urls* list or define the *start_request* function to generate the first pages. And make every function deal with the response instance and return a list of Items and Requests like this one:

    return [
        Request(
            following_url,
            headers = self.headers,
            callback = self.parse_following_page
        ),Request(
            fans_url,
            headers = self.headers,
            callback = self.parse_fans_page
        ), item
    ]

Scrapy use an asynchronization machenism to increase the effiency. Return the Request instance with call_back function and the Scheduler would schedule automatically.

Extracting links and data with certain pattern in HTML requires using XPath of Selector class.

	from scrapy.selector import Selector

	self.sel = Selector(response)

	# extract all user ids on the page
	uid_list = self.sel.xpath('//a/@href').re(r'.*uid=(\d+).*')

	# extract urls of friends' profile pages
	urls = self.sel.xpath('//table/tr/td[2]/a[1]/@href').extract()

	# extract next page url (下页 in Chinese)
	next_page = self.sel.xpath(u'//a[text()=\'下页\']/@href').extract()

	# extract number of commnets (评论 in Chinese)
	comments = self.sel.xpath('//a/text()').re(ur'^评论\[(\d+)\]')

######Run
The Item class definition requires almost no tricks. After building the Spider and Item class as well as customize the settings file, we have made a working crawler which could crawl and extract the target item as wanted.

Turn on the logging in Spider and run the crawler, we will see the details in LOG_FILE

	log.start(logfile='LOG_FILE')
	>scrapy crawl weibo

We could direct the crawler to output items in a json format to local file.

	>scrapy crawl weibo -o items.json -t json

Congratulations! If the .json file fulfills your data requirement, then your work has finished.
######ItemPipeline
Itempipeline is for filters and further export operations on result Items. Complicated export operations may also be defined in Item Exporters.

I stored my data in MySql database, so I make a connection with my db using twisted api and deal with tables here. Before the crawling, set up the database and create the tables. 

	from twisted.enterprise import adbapi

	self.dbpool = adbapi.ConnectionPool('MySQLdb', **dbargs)
	d = self.dbpool.runInteraction(self._do_upsert, item, spider)

	def _do_upsert(self, conn, item, spider):

Refer dirbot educational example for further details.

Now I have completed my crawler and store the results in Mysql database:)


####Problems & Solutions

######Request Priority
In Spider class, my extracted links include the **friends profile pages** and **next pages**. The next pages are due to the limit of total friends displaying in one page. I want my crawler to follow a bread-first method. Here the priority feature of Request Objects solves my problem. Just set **next pages** a higher priority than **friends profile pages**!

######Pause and Resume(failed)
Why I need to pause and resume the crawer?

- I want a larger dataset, as this is a link analysis on a social graph. My laptop usage are not sure to be insistant and our network is unstable.

- weibo has some prohibition that causes a frequent stop.

The official document provide [a way](http://doc.scrapy.org/en/latest/topics/jobs.html?highlight=pause) to do this by direct a JOBDIR to store the status of unfinished Requests and Request Filters. This requires you to keep serializable characteristic.

	>scrapy crawl weibo -s JOBDIR=cache/persistence
However, I failed to work with it. Maybe I have some problems with the serializable requirement. As I later found the data volume is passable for my project, I gave up this feature.

####Bug Shooting & Tips

1. Helpful references:
	- [regex](http://deerchao.net/tutorials/regex/regex.htm) 
	- [XPath](http://www.w3schools.com/XPath/)


2. If you want to use Mysql, install mysql\_python with easy\_install first.

