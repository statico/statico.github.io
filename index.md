---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.
  I'm an engineer at <a href="https://looker.com/">Looker</a>.
  Previously, I co-founded <a href="https://web.archive.org/web/http://www.artillery.com/news">Artillery</a>.
  See also:
  <a href="https://github.com/statico">GitHub</a>,
  <a href="https://twitter.com/statico">Twitter</a>,
  <a href="https://www.linkedin.com/in/ianlangworth">LinkedIn</a>.
  <a href="https://keybase.io/statico">Keybase</a>,
  <a href="https://github.com/statico/dotfiles">my dotfiles</a>,
  and <a href="https://langworth.com">langworth.com</a>.
</p>

<ul class="my-4">
  {% for post in site.posts %}
    {% if post.draft != true %}
      <li class="my-3">
        <a href="{{ post.url }}">{{ post.title }}</a>
        <span class="text-secondary">({{ post.date | date_to_string }})</span>
      </li>
    {% endif %}
  {% endfor %}
</ul>



