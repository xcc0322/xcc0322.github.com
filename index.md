---
layout: page
title: Welcome!
---
{% include JB/setup %}

![photo]({{ root_url }}assets/img/index_photo.jpg){: style="float:right;padding:30px;" width="125"}

This is Chengcheng.

I grew up in Qingdao, China and lived in Shanghai for 5 years.
Today, I am a software engineer in the San Francisco Bay Area.

I like coding, reading and spending time with my dog.

I put life blogs and learning snippets here.

<div class="blog-index">  
  {% assign post = site.posts.first %}
        {% if post.title %}
        <div class="section">
            <h1>Latest Post</h1>
            <a href="{{ root_url }}{{ post.url }}">{{ post.title }}</a>
        </div>
        {% endif %}
</div>
