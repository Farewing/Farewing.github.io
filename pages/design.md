---
layout: page
show_meta: false
title: "Style your content!"
subheadline: "Layouts of Farewing"
header:
   image_fullwidth: header_unsplash_11.jpg
permalink: "/design/"
---
<ul>
    {% for post in site.categories.design %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>