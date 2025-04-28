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
  <details>
    <summary><strong>{{ post.title }}</strong><br><span class="archive__item-excerpt">{{ post.excerpt }}</span></summary>
    <div class="project-content">
      {{ post.content }}
    </div>
  </details>
{% endfor %}

