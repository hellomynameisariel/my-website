---
layout: category
title: "Projects"
permalink: /projects/
taxonomy: projects
entries_layout: list
show_date: true
date_format: "%B %Y"
author_profile: true
---

{% for post in site.categories.projects %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p class="archive__item-excerpt">{{ post.excerpt }}</p>
{% endfor %}
