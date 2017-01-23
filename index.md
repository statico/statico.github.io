---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.
  I'm an engineer at <a href="https://looker.com/">Looker</a>.
  Previously, I co-founded <a href="https://www.crunchbase.com/organization/artillery">Artillery</a>.
  Email me at <code class="reverse">moc.htrowgnal@nai</code>.
</p>

<table class="table table-bordered">
  <tr>
    <th>Date</th>
    <th>Title</th>
  </tr>
  {% for post in site.posts %}
    {% if post.draft != true %}
      <tr>
        <td>{{ post.date | date_to_string }}</td>
        <td><a href="{{ post.url }}">{{ post.title }}</a></td>
      </tr>
    {% endif %}
  {% endfor %}
</table>

<p>
  See also:
  <a href="https://github.com/statico">GitHub</a>,
  <a href="https://twitter.com/statico">Twitter</a>,
  <a href="https://www.linkedin.com/in/ianlangworth">LinkedIn</a>
</p>

