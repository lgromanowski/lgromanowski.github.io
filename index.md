{%- for post in site.posts -%}
<li>
  {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
  <span class="post-meta">
    {%- if post.last_modified_at -%}
      {{ post.date | date: date_format }}, last updated {{ post.last_modified_at | date: date_format }}
    {%- else -%}
      {{ post.date | date: date_format }}
    {%- endif -%}
  </span>
  <h3>
    <a class="post-link" href="{{ post.url | relative_url }}">
      {{ post.title | escape }}
    </a>
  </h3>
  {%- if site.show_excerpts -%}
    {{ post.excerpt }}
  {%- endif -%}
</li>
{%- endfor -%}
