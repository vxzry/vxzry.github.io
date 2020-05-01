---
layout: page
title: Search
permalink: /search/
---
<div id="search-container">
    <input type="text" id="search-input" placeholder="Search through the blog posts...">
</div>
    
<div id="archives">
Categories: 
{% for category in site.categories %}
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <a class="text-muted text-small" href="{{site.baseurl}}/categories/#{{category_name|slugize}}">{{ category_name }}</a>
    {% unless forloop.last %}&comma;&nbsp;{% endunless %}
{% endfor %}
</div>

<div class="container ">
    <div class="row">
        <ul id="results-container"></ul>
    </div>
</div>

<script src="{{ site.baseurl }}/assets/simple-jekyll-search.min.js" type="text/javascript"></script>

<script>
    SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        searchResultTemplate: `
        <div class="text-left mt-3">
            <a class="text-dark" href="{url}"><h4 class=" mb-0 pb-0">{title}</h4></a>
            <span class="text-muted">{date}</span>
        </div>`,
        json: '{{ site.baseurl }}/search.json',
        fuzzy: false
    });
</script>