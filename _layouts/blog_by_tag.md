---
layout: page
---

<h1>Articles related to {{ page.tag }}</h1>
<div>
    {% if site.tags[page.tag] %}
        {% for post in site.tags[page.tag] %}
            <a href="{{ post.url }}/">{{ post.title }}</a>
            <br>
        {% endfor %}
    {% else %}
        <p>There are no posts for this tag.</p>
    {% endif %}
</div>