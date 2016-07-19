---
layout: page
title: Fluo Release Notes Archive
permalink: "/release-notes/"
redirect_to: "{{ site.apache }}{{ page.url }}"
---

Below are the release notes for all Fluo releases:

{% for release in site.categories.release-notes %}
* [{{ release.version }}]({{ site.baseurl }}/release-notes/{{ release.version }}/) - {{ release.date | date_to_string }}
{% endfor %}
