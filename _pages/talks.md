---
layout: archive
title: "Talks and presentations"
permalink: /talks/
author_profile: true
---

{% if site.talkmap_link == true %}

<p style="text-decoration:underline;"><a href="/talkmap.html">See a map of all the places I've given a talk!</a></p>

{% endif %}

{% for post in site.talks reversed %}
  {% include archive-single-talk.html %}
{% endfor %}

* WiAdv: Practical and Robust Adversarial Attack against WiFi-based Gesture Recognition System.
    * ACM IMWUT/Ubicomp 2022, Cambridge, UK [[event link]](https://ubicomp.org/ubicomp2022/) [[video]](https://youtu.be/J-l1U5XGXH8)
 
* RIStealth: Practical and Covert Physical-Layer Attack against WiFi-based Intrusion Detection via Reconfigurable Intelligent Surface
    * ACM SenSys 2023, Istanbul, Turkyie [[event link]](https://sensys.acm.org/2023/) [[video]](https://youtu.be/d3_PVTRhQh4)
