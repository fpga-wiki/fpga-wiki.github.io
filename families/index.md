---
layout: page
title: FPGA Device Family Index
excerpt: "An archive of posts sorted by families."
search_omit: true
---

{% capture site_families %}{% for family in site.families %}{{ family | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign families_list = site_families | split:',' | sort %}

<ul class="family-box inline">
  {% for item in (0..site.families.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ families_list[item] | strip_newlines }}{% endcapture %}
    <li><a href="#{{ this_word }}">{{ this_word }} <span>{{ site.families[this_word].size }}</span></a></li>
  {% endunless %}{% endfor %}
</ul>

{% for item in (0..site.families.size) %}{% unless forloop.last %}
  {% capture this_word %}{{ families_list[item] | strip_newlines }}{% endcapture %}
  <h2 id="{{ this_word }}">{{ this_word }}</h2>
  <ul class="post-list">
  {% for post in site.families[this_word] %}{% if post.title != null %}
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}<span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span></a></li>
  {% endif %}{% endfor %}
  </ul>
{% endunless %}{% endfor %}
