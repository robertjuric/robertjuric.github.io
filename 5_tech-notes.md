---
layout: default
title: Tech-Notes
permalink: /tech-notes/
---

<div class="container" >
<div id="archives">
<p>
I'm excited to finally be moving all of my technical articles to my new blog, [Directly Connected](https://directlyconnected.wordpress.com/). Starting in 2017 all new content will be posted to my Directly Connected blog. As I migrate older articles over they will be removed from this list. This should help clean up RSS feed issues and provide clean deliniation between personal and work articles.<br>
<br>
all technical articles<br>
(or browse by <a title="The complete archive of {{ site.name }} by category" href="{{ site.url}}{{site.baseurl}}/categoryview">category</a>)</p>
</div>
</div>

<ul class="post-list">
{% for post in site.posts %}
    {% if post.categories[0] == "Blog" %}
    
    {% else if %}
    <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}, {{ post.categories[0] }}</span>
        <h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
    </li>
    {% endif %}  
{% endfor %}
</ul>
<p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>
  
  
