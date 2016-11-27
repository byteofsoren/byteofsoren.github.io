---
layout: page
title: Robotic student online.
tagline:
---
{% include JB/setup %}
{% site.page = site.page + "about.md" %}

# Introduction
Hello and welcome to my blog about robotics, programming and mathematics.
My name is Magnus Sörensen and I'm studding robotics in Märlardalens högskola
Västerås. This page is a kind of diary to track and store useful information
and there fore may contain technical information without any or non-existent
information. For deaper information about me check out the about page.
[About me](about.md)


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


