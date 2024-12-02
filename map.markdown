---
layout: single
title: Resource map
permalink: /map/
---
Below you will find a mapping of statute sections to technical resources. As more resources are added, this map will be updated. 
{% mermaid %}
graph LR
    %% Style definitions
    classDef statute fill:#e6f3ff,stroke:#4a4a4a,stroke-width:2px,cursor:pointer
    classDef tech fill:#f9f9f9,stroke:#666,stroke-width:1px,cursor:pointer

    {% for statute in site.data.statute-tech-links %}
    {{ statute[0] }}["{{ statute[1].section }}: {{ statute[1].name }}"]
    click {{ statute[0] }} "{{ statute[1].url }}" "Click to view {{ statute[1].name }}"
    {% endfor %}

    {% assign tech_resources = "" | split: "" %}
    {% for statute in site.data.statute-tech-links %}
      {% if statute[1].links %}
        {% for link in statute[1].links %}
          {% assign tech_id = link.url | slugify %}
          {% unless tech_resources contains tech_id %}
            {{ tech_id }}["{{ link.text }}"]
            {% assign tech_resources = tech_resources | push: tech_id %}
          {% endunless %}
          {{ statute[0] }} --> {{ tech_id }}
          click {{ tech_id }} "{{ link.url }}" "Click to view {{ link.text }}"
        {% endfor %}
      {% endif %}
    {% endfor %}

    {% for statute in site.data.statute-tech-links %}
    class {{ statute[0] }} statute
    {% endfor %}
    {% for tech_id in tech_resources %}
    class {{ tech_id }} tech
    {% endfor %}
{% endmermaid %}