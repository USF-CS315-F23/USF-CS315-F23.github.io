---
layout: home
title: Home
nav_order: 1
#nav_exclude: true
permalink: /:path/
seo:
  type: Course
  name: CS 315 Operating Systems Fall 2023
---

# CS 315 Operating Systems
Spring 2023 &nbsp; &nbsp; &nbsp; &nbsp; [Sec 01 Zoom](Join Zoom Meeting
https://usfca.zoom.us/j/83086960997)  &nbsp; &nbsp; [Sec 02 Zoom](https://usfca.zoom.us/j/83443279350)

{% for module in site.modules %}
{{ module }}
{% endfor %}
