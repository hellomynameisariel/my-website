---
layout: default
title: "Projects"
permalink: /projects/
author_profile: true
css: /assets/css/sidepanel.css
---

<style>
  /* Ensure the navigation bar is fixed and styled appropriately */
  #site-nav.greedy-nav {
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: nowrap;
    padding: 0.5rem 1rem;
    position: fixed;
    top: 0;
    width: 100%;
    background: white;
    z-index: 1000;
    overflow: hidden;
  }

  /* Style for the site title */
  #site-nav .site-title {
    flex-shrink: 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  /* Container for visible navigation links */
  #site-nav .visible-links {
    display: flex;
    gap: 1rem;
    margin-left: auto;
    list-style: none;
    padding-left: 0;
    overflow: hidden;
    white-space: nowrap;
  }

  /* Individual navigation items */
  #site-nav .masthead__menu-item a {
    text-decoration: none;
    color: hotpink;
    white-space: nowrap;
  }

  /* Prevent horizontal scrolling */
  html, body {
    overflow-x: hidden;
  }

  /* Push content below the fixed navigation bar */
  body {
    padding-top: 60px;
  }

  /* Styles for the projects list */
  .projects-list {
    padding-left: 2rem;
    max-width: 55%;
  }

  /* Sticky year headings */
  .year-heading {
    position: sticky;
    top: 60px;
    background: white;
    padding: 0.5rem 0;
    z-index: 1;
  }

  /* Project excerpt styling */
  .project-excerpt {
    margin-top: 0.5rem;
    margin-bottom: 1.5rem;
    line-height: 1.6;
    color: #555;
  }

  /* Overlay styling */
  .overlay {
    position: fixed;
    top: 60px;
    left: 0;
    width: 100%;
    height: calc(100% - 60px);
    background: rgba(0, 0, 0, 0.3);
    z-index: 999;
    display: none;
    transition: opacity 0.3s ease;
  }

  body.flyout-open .overlay {
    display: block;
  }
</style>


<div class="projects-list">
  {% assign project_posts = site.pages | where_exp: "page", "page.categories contains 'projects'" %}
  {% assign project_posts = project_posts | sort: "date" | reverse %}

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
      <a href="/my-website{{ post.permalink }}" class="project-title-link"><strong>{{ post.title }}</strong></a>
      <p class="project-excerpt">{{ post.excerpt }}</p>
    </div>
  {% endfor %}
</div>

<div id="overlay" class="overlay hidden"></div>

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
  ">X [esc]</button>
  <div id="panel-content">Loading...</div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const body = document.body;
  const overlay = document.getElementById('overlay');

  document.querySelectorAll('.project-link').forEach(link => {
    link.addEventListener('click', function(event) {
      event.preventDefault();
      const url = this.getAttribute('data-project-url');

      document.getElementById('panel-content').innerHTML = "Loading...";
      document.getElementById('side-panel').classList.remove('hidden');
      overlay.classList.remove('hidden');
      body.classList.add('flyout-open');

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

  function closePanel() {
    document.getElementById('side-panel').style.transform = 'translateX(100%)';
    document.getElementById('side-panel').classList.add('hidden');
    overlay.classList.add('hidden');
    body.classList.remove('flyout-open');
  }

  document.getElementById('close-panel').addEventListener('click', closePanel);

  document.addEventListener('keydown', function(event) {
    if (event.key === "Escape") {
      closePanel();
    }
  });

  overlay.addEventListener('click', closePanel);
});
</script>
