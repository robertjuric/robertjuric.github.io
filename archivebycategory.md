---
layout: page
permalink: /categoryview/
sitemap: false
---
   <div class="container" >
         <div id="archives">
             browse by <a title="The complete archive of {{ site.name }}'s Blog"
                          href="{{ site.url}}{{site.baseurl}}/tech-notes">latest</a>
         </div>
     </div>

<div>
  {% assign categories = site.categories | sort %}
  {% for category in categories %}
  {% if category[0] == "Blog" %}

  {% else if %}
   <span class="site-tag">
      <a href="#{{ category | first | slugify }}">
              {{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})
        </a>
    </span>
    {% endif %} 
    {% endfor %}
    </div>
    
  <div id="index">
    {% for category in categories %}
    {% if category[0] == "Blog" %}
    // skip it
    {% else if %}
    <a name="{{ category[0] }}"></a><h2>{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }}) </h2>
    {% assign sorted_posts = site.posts | sort: 'title' %}
    {% endif %} 
    {% for post in sorted_posts %}
    {%if post.categories contains category[0]%}
    {% if category[0] == "Blog" %}

    {% else if %}
    <h3><a href="{{ site.url }}{{site.baseurl}}{{ post.url }}" title="{{ post.title }}">{{ post.title }} <p class="date">{{ post.date |  date: "%B %e, %Y" }}</p></a></h3>
    
  {% endif %}  
  {%endif%}
  {% endfor %}

  {% endfor %}
</div>