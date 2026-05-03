---
layout: single
title: "Categories"
permalink: /categories-list/
---

<style>
.page__title {
  display: none;
}
</style>

# Categories

{% for category in site.categories %}
## {{ category[0] }}

<ul>
  {% for post in category[1] %}
    <li><a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%Y-%m-%d" }}</li>
  {% endfor %}
</ul>
{% endfor %}