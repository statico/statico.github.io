---
layout: page
title: Welcome
---

<h2>Welcome</h2>

<p>
  I'm Ian. I build things.  
  I co-founded <a href="https://artillery.com">Artillery</a>.
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

