---
layout: page
permalink: /teaching/
title: Teaching
description: Courses' List
nav: true
---

{% assign coursesByYear = site.courses | group_by: "year" %}

{% for year in coursesByYear reversed %}

<h1>
    <span onclick="hideShow({{year.name}})">
        <div class="teaching-year">
            {{ year.name }}/{{ year.name | plus: 1 }}
        </div>
    </span>
</h1>

{% assign postYear = year.name | plus: 0 %}
{% assign postYear1 = year.name | plus: 1 %}
{% assign nowYear = site.time | date: "%Y" | plus: 0 %}
{% assign nowMonth = site.time | date: "%m" | plus: 0 %}

{% if nowYear == postYear and nowMonth > 8 or nowYear==postYear1 and nowMonth<=8 %}

<ul id={{ year.name  }}>

{% else %}

<ul id={{ year.name  }} style="display:none">

{% endif %}

{% for course in year.items %}

<li>

<h3>
    <a class="teaching-title" href="{{ course.slug }}">
        {{ course.title }}
    </a>
    &nbsp;
    <a href="{{ course.slug  }}.xml" class="teaching-rss-icon">
        <i class="fas fa-rss"></i>
    </a>
</h3>

{% if course.description %}

<p>{{ course.description }}</p>

{% endif %}

</li>

{% endfor %}

</ul>

{% endfor %}

<script>
function hideShow(year) {
  var x = document.getElementById(year);
  if (x.style.display === "none") {
    x.style.display = "block";
  } else {
    x.style.display = "none";
  }
}
</script>

