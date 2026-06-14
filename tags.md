---
layout: page
title: Tags
permalink: /tags/
---

{% assign sorted_tags = site.tags | sort %}
<div class="tag-cloud">
  {% for tag in sorted_tags %}<a class="tag" href="#{{ tag[0] | slugify }}">{{ tag[0] }} <span>{{ tag[1] | size }}</span></a>{% endfor %}
</div>

{% for tag in sorted_tags %}
<section class="tag-group" id="{{ tag[0] | slugify }}">
  <h2>{{ tag[0] }}</h2>
  <ul class="post-list compact">
    {% for post in tag[1] %}
    <li class="post-item">
      <time class="post-date" datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: '%-d %b %Y' }}</time>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ul>
</section>
{% endfor %}
