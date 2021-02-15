---
title: ""
tagName: book_reviews
search: exclude
permalink: tag_book_reviews.html
sidebar: none
toc: false
folder: tags
---
<!-- {% include taglogic.html %} -->
<h4>Tag Book Reviews</h4>
<br/>
<ul>
{% for page in site.pages %}
{% for tag in page.tags %}
{% if tag == "book_reviews" %}
<li><a href="{{page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>
{% include links.html %}
