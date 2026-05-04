---
layout: single
title: "文章分类"
permalink: /categories-list/
---

<style>
.page__title {
  display: none;
}
</style>

<h1>文章分类</h1>

{% for category in site.categories %}
## {{ category[0] }}

<ul>
  {% for post in category[1] %}
    <li><a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%Y-%m-%d" }}</li>
  {% endfor %}
</ul>
{% endfor %}