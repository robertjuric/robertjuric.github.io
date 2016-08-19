---
layout: page
title: Tech-Notes
permalink: /tech-notes/
---

<div>
  {% assign categories = site.categories | sort %}
  {% for category in categories %}
   <span class="site-tag">
      <a href="#{{ category | first | slugify }}">
              {{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})
        </a>
    </span>
    {% endfor %}
    </div>
    
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

        <h2>
          <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
        </h2>
      </li>
    {% endfor %}
  </ul>
  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>