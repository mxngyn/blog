---
layout: default
title: Archive
---

<section id="archive">
<h2>Archive</h2>
{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ](blog/{{ post.url }})
{% endfor %}
</section>