---
layout: post
title: "Displaying an index of posts"
categories: update
author:
- Bart Simpson
- Nelson Mandela Muntz
meta: "Springfield"
---
Displaying an index of posts
Creating an index of posts on another page should be easy thanks to Liquid and its tags. Here’s a simple example of how to create a list of links to your blog posts:

```html
{% for category in site.categories %}
  {% if category[0] == "update" %}
  <h3>{{ category[0] }}</h3>
  {% endif %}
  {% if category[0] == "update" %}
    <ul>
      {%- for post in category[1] -%}
      <li>
        <span class="post-meta">{{ post.date | date: date_format }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
      </li>
      {%- endfor -%}
    </ul>
  {% endif %}
{% endfor %}
```
You have full control over how (and where) you display your posts, and how you structure your site. You should read more about how templates work with Jekyll if you want to know more.
CategoriesPermalink
Categories of a post work similar to the tags above:

They can be defined via the front matter using keys category or categories (that follow the same logic as for tags)
All categories registered in the site are exposed to Liquid templates via site.categories which can be iterated over (similar to the loop for tags above.)