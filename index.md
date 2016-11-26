---
layout: page
title: Robotic student online.
tagline:
---
{% include JB/setup %}

# Introduction
Hello and welcome to my blog about robotics, programming and mathematics.
My name is Magnus Sörensen and I'm studding robotics in Märlardalens högskola
Västerås. This page is a kind of diary to track and store useful information
and there fore may contain technical information without any or non-existent
information.

## Courses i read or have finished.
# Year 1
<ul>
    <li> Vector algebra </li>
    <li> Single variable analytics (not finished)</li>
    <li> CAD in robotics</li>
    <li> Intro to programming in C</li>
    <li>Mechanics 1</li>
    <li> Data structures</li>
</ul>
-- Year 2
. Multi variable calculus
. MATLAB
. Vector algebra (doing now)
. Mechanics 2 (doing now)


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


