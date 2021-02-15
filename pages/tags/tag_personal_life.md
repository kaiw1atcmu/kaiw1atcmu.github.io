---
title: ""
tagName: personal_life
search: exclude
permalink: tag_personal_life.html
sidebar: none
toc: false
folder: tags
---
<!-- {% include taglogic.html %} -->
<h4>Tag Personal Life</h4>
<br/>
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "personal_life" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}
