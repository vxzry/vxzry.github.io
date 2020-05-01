---
layout: page
permalink: /categories/
title: Categories
---


<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div ></div>
    <p></p>    
    <h4 id="#{{ category_name | slugize }}" class="category-head">{{ category_name }} 
      <span>({{site.categories[category_name].size}})</span>
    </h4>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      - <a class="text-muted" href="{{ site.baseurl }}{{ post.url }}">{% if post.title and post.title != "" %}{{post.title}}{% else %}{{post.excerpt |strip_html}}{%endif%}</a>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>