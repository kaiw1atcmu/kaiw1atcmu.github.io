---
title: ""
tagName: foreign_languages
search: exclude
permalink: tag_foreign_languages.html
sidebar: none
toc: false
folder: tags
---
<!-- {% include taglogic.html %} -->
<h4>Tag Foreign Languages</h4>
<br/>
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "foreign_languages" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}