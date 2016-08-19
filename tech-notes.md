---
layout: default
title: Tech-Notes
permalink: /tech-notes/
---

   <div class="container" >
         <div id="archives">
             browse by <a title="The complete archive of {{ site.name }}'s Blog by category"
                          href="{{ site.url}}{{site.baseurl}}/categoryview">category</a>
         </div>
     </div>

   
  <ul class="post-list">
    {% for post in site.posts %}
    {% if post.categories[0] == "Blog" %}
    
    {% else if %}
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
  
  
