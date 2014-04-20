---
layout: post
title: "WeiboSpammer-Chapter0 Introduction"
description: ""
category: cs
tags: [Weibo Spammer, CS course]
---
{% include JB/setup %}


![weibo_logo](http://blogs-images.forbes.com/rebeccafannin/files/2014/04/5812606276_2401e9c0f9_b.jpg){: style="display: block;margin-left: auto;margin-right:auto;width:100%"}

This is the project for my data mining course this semester. 

The course is based on [this textbook](http://infolab.stanford.edu/~ullman/mmds/book.pdf) by Stanford University. The contents are well strutured and very pratical.

####The Spammer Problem Description

Recently there's a spammer problem with weibo. Some tricky guys make spammer farms and sell the fans number increasement for those who want to increase their popularity in a short time. These spammers are not like the traditional zombie accounts. They have normal head portraits and make interaction actively with true users. Some of them follow each other. Some follow users randomly. The master guy sell it online like in taobao.com.

I observed this because every day I gained some zombie fans which is kind of annoying. One of my roommates start a shop online recently and use the spammers to do a propagation.

After the lecture of link analysis, I thought the algorithm on spam detection might work with this spammer problem.

####The Algorithm
I proposed the project to my teacher and planned to deploy the treatment with link spam. The teacher suggested two further directions and gave references to two papers. [SNARE](http://www.cs.cmu.edu/~mmcgloho/pubs/snare.pdf) and [SMFSR](http://www.cs.ust.hk/~qyang/Docs/2012/AAAI_2012_Spammers.pdf). They are about link analysis on anomaly detection and matrix factorization respectively. After reading them, I found the first one fits my problem better and decided to implement it.

####The Data
The first step is to get my data. I mainly need two part of data:

- **links** the fans and followed weiboers pairs

- **user profile** the profile to decide a starting value of anomaly

It's hard to find fit dataset. Immediately I turn to crawling.

My friends suggested crawling with weibo API. I had a try but later found the frequency and user authentication are too limited for my need. So we have to extract what we want from html pages directly. I played with Scrapy and found it worked.

####The Schedule

4.15 propose and start  

4.20 data crawling finised  

4.21-5.15 algorithm implementation  

5.15-5.30 result anaylisis and report  

5.30 deadline


####About the posts
This is my first technical post on this blog. I wrote in a mixed style of recording and tutorial. The main target is to help myself understand the process better. I'll be happy if it helps readers in a way of bug shooting or inspiration.
