---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.  
  You can find me on
  <a href="https://github.com/statico">GitHub</a>,
  <a href="http://twitter.com/statico">Twitter</a>,
  <a href="http://www.linkedin.com/in/ianlangworth">LinkedIn</a>,
  and <a href="http://flickr.com/photos/statico">Flickr</a>.
  Email me at <code class="reverse">moc.htrowgnal@nai</code>.
</p>

<ul class="posts">
  {% for post in site.posts %}
    {% if post.draft != true %}
      <li>
        <span>{{ post.date | date_to_string }}</span> &raquo;
        <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>

