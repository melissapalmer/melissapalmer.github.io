---
layout: default
---

<div class="well article">
{% for tips in site.tipsandtricks %}
  <h2>
    <a href="{{ tips.url }}">
      {{ tips.description }}
    </a>
  </h2>
  <p>{{ tips.content | markdownify }}</p>
{% endfor %}
</div>