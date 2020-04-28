---
layout: page
title: Projects
permalink: /projects
---

{%- if site.projects.size > 0 -%}
  <ul class="post-list">
  {%- for project in site.projects -%}
  <li>
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    {{ project.content }}
  </li>
  {%- endfor -%}
  </ul>
{%- endif -%}
