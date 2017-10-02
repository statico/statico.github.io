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

<h4>On this blog:</h4>

<ul class="my-4">
  {% for post in site.posts %}
    {% if post.draft != true %}
      <li class="my-3">
        <a href="{{ post.url }}">{{ post.title }}</a>
        <span class="text-secondary">- {{ post.date | date_to_string }}</span>
      </li>
    {% endif %}
  {% endfor %}
</ul>

<h4>Published elsewhere:</h4>

<ul class="my-4">

<li class="my-3">
<a href="http://firstround.com/review/How-to-Go-From-Google-Engineer-to-First-Time-CTO/">How to Go From Google Engineer to First-Time CTO</a>
<br/><span class="text-secondary">First Round Review, 2015</span>
</li>

<li class="my-3">
<a href="http://shop.oreilly.com/product/9780596100926.do">Perl Testing: A Developer's Notebook</a>
<br/><span class="text-secondary">O'Reilly Media, 2005</span>
</li>

</ul>



