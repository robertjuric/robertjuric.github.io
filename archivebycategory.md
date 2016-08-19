---
layout: page
permalink: /categoryview/
sitemap: false
---
   


<div>
{% assign categories = site.categories | sort %}
{% for category in categories %}
    {% if category[0] == "Blog" %}
    {% else if %}
        <a href="#{{ category | first | slugify }}">{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})</a>
    {% endif %} 
{% endfor %}
</div>

<div class="container" >
<div id="archives">
<p>(or browse by <a title="The complete archive of {{ site.name }}" href="{{ site.url}}{{site.baseurl}}/tech-notes">latest)</a></p>
</div>
</div>
    
<div id="index">
{% for category in categories %}
    {% if category[0] == "Blog" %}
    {% else if %}
        <a name="{{ category }}"></a><h2>{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }}) </h2>
        {% assign sorted_posts = site.posts | sort: 'last' %}
    {% endif %} 
    {% for post in sorted_posts %}
        {%if post.categories contains category[0]%}
            {% if category[0] == "Blog" %}
            {% else if %}
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3><a href="{{ site.url }}{{site.baseurl}}{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></h3>
            {% endif %}  
        {%endif%}
  {% endfor %}
{% endfor %}
</div>