---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.
  I'm the CTO of <a href="https://www.wilburlabs.com/">Wilbur Labs</a> in San Francisco, California.
  See also:
  <a href="https://www.linkedin.com/in/ianlangworth">LinkedIn</a>.
  <a href="https://github.com/statico">GitHub</a>,
  <a href="https://twitter.com/statico">Twitter</a>,
  <a href="https://keybase.io/statico">Keybase</a>,
  <a href="https://github.com/statico/dotfiles">my dotfiles</a>,
  and <a href="https://langworth.com">langworth.com</a>.
</p>

<h3>On this blog:</h3>

<ul class="my-4">
  {% for post in site.posts %}
    {% if post.draft != true %}
      <li class="my-3">
        <a href="{{ post.url }}">{{ post.title }}</a>
        <br/><span class="text-secondary">{{ post.date | date_to_string }}</span>
      </li>
    {% endif %}
  {% endfor %}
</ul>

<h3>Published elsewhere:</h3>

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



