---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.
  I'm an engineer at <a href="https://looker.com/">Looker</a>.
  Previously, I co-founded <a href="https://web.archive.org/web/http://www.artillery.com/news">Artillery</a>.
  Email me at <code class="reverse">moc.htrowgnal@nai</code>.
  See also:
  <a href="https://github.com/statico">GitHub</a>,
  <a href="https://twitter.com/statico">Twitter</a>,
  <a href="https://www.linkedin.com/in/ianlangworth">LinkedIn</a>.
  <a href="https://keybase.io/statico">Keybase</a>,
  <a href="https://github.com/statico/dotfiles">my dotfiles</a>.
</p>

<ul class="my-4">
  {% for post in site.posts %}
    {% if post.draft != true %}
      <li class="my-3">
        <a href="{{ post.url }}">{{ post.title }}</a>
        ({{ post.date | date_to_string }})
      </li>
    {% endif %}
  {% endfor %}
</ul>



