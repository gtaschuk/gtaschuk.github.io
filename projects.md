---
layout: page
title: Projects
permalink: /projects
---

{%- if site.projects.size > 0 -%}
{% assign sorted = (site.projects | sort: 'date') | reverse %}
  <ul class="post-list">
  {%- for project in sorted -%}
  <li style="border: 0px solid gray; padding: 10px">
    <h2>
    {%- if project.external -%}
    <a href="{{project.external}}">
      {{project.title}}
    </a>
    {%- else -%}
    {{project.title}}
    {%- endif -%}
    {%- if project.github -%}
    <a href="{{project.github}}">
      <img src="/assets/GitHub-Mark-64px.png" width="32" height="*"/>
    </a>
    {%- endif -%}
    </h2>
    <div style="margin-left: 30px">
    {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
    {{ project.content }}
    </div>
  </li>
  {%- endfor -%}
  </ul>
{%- endif -%}
