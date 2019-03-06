---
layout: default
---

<div class="well article">
{% for tips in site.tipsandtricks %}
    <a href="{{ tips.url }}">
      {{ tips.description }}
    </a>
{% endfor %}
</div>