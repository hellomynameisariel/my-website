---
layout: default
title: "Projects"
permalink: /projects/
author_profile: true
css: /assets/css/sidepanel.css
---

<style>
.projects-list {
  padding-left: 2rem;
  max-width: 55%;
}

.year-heading {
  position: sticky;
  top: 60px;
  background: white;
  padding: 0.5rem 0;
  z-index: 1;
}
</style>

<div class="projects-list" style="padding-left: 2rem; max-width: 55%;">
  {% assign project_posts = site.pages | where_exp: "page", "page.categories contains 'projects'" %}
  {% assign current_year = "" %}
  {% assign current_month = "" %}
  {% for post in project_posts %}
    {% assign post_year = post.date | date: "%Y" %}
    {% assign post_month = post.date | date: "%B" %}

  {% if post_year != current_year %}
    <h2 class="year-heading">{{ post_year }}</h2>
    {% assign current_year = post_year %}
  {% endif %}

  {% if post_month != current_month %}
    <h3>{{ post_month }}</h3>
    {% assign current_month = post_month %}
  {% endif %}

    <div class="project-link" data-project-url="/my-website{{ post.permalink }}">
      <strong><a href="/my-website{{ post.permalink }}" class="project-title-link">{{ post.title }}</a></strong><br>
      <span>{{ post.excerpt }}</span>
    </div>
  {% endfor %}
</div>

<div id="side-panel" class="hidden" style="
  position: fixed;
  top: 60px;
  right: 0;
  width: 40%;
  height: calc(100% - 60px);
  background: white;
  box-shadow: -2px 0 5px rgba(0,0,0,0.1);
  overflow-y: auto;
  padding: 20px;
  z-index: 9999;
  transform: translateX(100%);
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
">
  <button id="close-panel" style="
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 20px;
    background: none;
    border: none;
    cursor: pointer;
  ">X</button>
  <div id="panel-content">Loading...</div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('.project-link').forEach(link => {
    link.addEventListener('click', function(event) {
      event.preventDefault();
      const url = this.getAttribute('data-project-url');

      document.getElementById('panel-content').innerHTML = "Loading...";
      document.getElementById('side-panel').classList.remove('hidden');

      fetch(url)
        .then(response => response.text())
        .then(html => {
          const parser = new DOMParser();
          const doc = parser.parseFromString(html, 'text/html');
          const content = doc.querySelector('.project-content');

          if (content) {
            document.getElementById('panel-content').innerHTML = content.innerHTML;
          } else {
            document.getElementById('panel-content').innerHTML = "Content not found.";
          }
          document.getElementById('side-panel').style.transform = 'translateX(0)';
        })
        .catch(error => {
          document.getElementById('panel-content').innerHTML = "Error loading content.";
          document.getElementById('side-panel').style.transform = 'translateX(0)';
        });
    });
  });

  document.querySelectorAll('.project-title-link').forEach(link => {
    link.addEventListener('click', function(event) {
      event.preventDefault();
      this.closest('.project-link').click();
    });
  });

  document.getElementById('close-panel').addEventListener('click', function() {
    document.getElementById('side-panel').style.transform = 'translateX(100%)';
    document.getElementById('side-panel').classList.add('hidden');
  });

  document.addEventListener('keydown', function(event) {
    if (event.key === "Escape") {
      document.getElementById('side-panel').style.transform = 'translateX(100%)';
      document.getElementById('side-panel').classList.add('hidden');
    }
  });
});
</script>
