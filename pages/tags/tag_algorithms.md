---
title: "Algorithms pages"
tagName: algorithms
search: exclude
permalink: tag_algorithms.html
sidebar: none
folder: tags
---
<!-- {% include taglogic.html %} -->
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "algorithms" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}
