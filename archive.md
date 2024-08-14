---
layout: page
title: Blog Archive
---
<!-- {% capture tags %}
  {% for tag in site.tags %}
    {{ tag[0] }}
  {% endfor %}
{% endcapture %}
{% assign sortedtags = tags | split:' ' | sort %}

{% for tag in sortedtags %} -->
{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.date | date: "%d %b %Y" }} - {{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}