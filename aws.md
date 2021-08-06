---
layout: default
---
<div class="well article">
    <div class="post-content">
        <h1>AWS</h1>        
      
        <hr style="border-top:1px solid #28323C;"/>
        A collection of AWS related bits and bobs
        <hr style="border-top:1px solid #28323C;"/>        
    </div>
</div>
<br/>

<div class="well article">
{% for a in site.aws %}   
      <ul>       
        <li>
              <a href="{{ a.url }}">
                {{ a.description }}
                </a>    
       </li> 
    </ul>
{% endfor %}
</div>