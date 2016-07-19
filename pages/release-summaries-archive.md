---
layout: page
title: Fluo Release Summaries Archive
permalink: "/release-summaries/"
redirect_to: "{{ site.apache }}{{ page.url }}"
---

Below are the release summaries for all Fluo releases:

{% for release in site.categories.release-summaries %}
* [{{ release.version }}]({{ site.baseurl }}/release-summaries/{{ release.version }}/) - {{ release.date | date_to_string }}
{% endfor %}
