---
title: ""
tagName: data_structures
search: exclude
permalink: tag_data_structures.html
sidebar: none
toc: false
folder: tags
---
<!-- {% include taglogic.html %} -->
<h4>Tag Data Structures</h4>
<br/>
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "data_structures" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}
