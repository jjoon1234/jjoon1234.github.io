---
layout: default
title: "프로젝트"
---

## 프로젝트 목록

<ul class="project-list">
  {% for post in site.posts %}
    {% if post.category == 'project' %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        <span>{{ post.date | date_to_string }}</span>
      </li>
    {% endif %}
  {% endfor %}
</ul>
