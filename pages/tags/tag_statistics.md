---
title: ""
tagName: statistics
search: exclude
permalink: tag_statistics.html
sidebar: none
toc: false
folder: tags
---
<!-- {% include taglogic.html %} -->
<h4>Tag Statistics</h4>
<br/>
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "statistics" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}