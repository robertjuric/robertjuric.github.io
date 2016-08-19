---
layout: default
title: Blog
permalink: /blog/
---

<p class="intro">These posts are part of my personal blog, a place for my random thoughts and feelings when I feel like expressing them.</p>
<ul class="post-list">
{% for post in site.posts %}
  {% if post.categories[0] == "Blog" %}
  <li>
    <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
    <h2>
    <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    </h2>
  </li>
  {% endif %}  
{% endfor %}
</ul>
<p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>
  
  
