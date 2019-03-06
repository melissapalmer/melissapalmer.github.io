---
layout: default
---
<div class="well article">
    <div class="post-content">
        <h1>Various Tips and Tricks</h1>        
      
        <hr style="border-top:1px solid #28323C;"/>
        A collection of tips and ticks for various technologies
        <hr style="border-top:1px solid #28323C;"/>        
    </div>
</div>
<br/>

<div class="well article">
{% for tips in site.tipsandtricks %}
    <a href="{{ tips.url }}">
      {{ tips.description }}
    </a> </br>
{% endfor %}
</div>