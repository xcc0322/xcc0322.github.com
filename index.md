---
layout: page
title: Welcome!
---
{% include JB/setup %}

![photo](http://hdn.xnimg.cn/photos/hdn521/20130714/1525/large_qutA_ec0300055ffa113e.jpg){: style="float:right;padding:30px;" width="125"}

Nice to meet you. 

I'm Chengcheng, a software engineer. I grew up in Qingdao and now live in Shanghai. I like **coding** and  **reading**. I put some knowledge notes, blogs and snippets here.

<div class="blog-index">  
  {% assign post = site.posts.first %}
        {% if post.title %}
        <div class="section">
            <h1>Latest Post</h1>
            <a href="{{ root_url }}{{ post.url }}">{{ post.title }}</a>
        </div>
        {% endif %}
</div>

