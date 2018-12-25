---
layout: page-fullwidth
title: "Passing a list as parameters to scripts"
subheadline: "Passing list to Scripts from Automations"
meta_teaser: "Want to pass list items as parameters to the scripts?"
teaser: "Here is a simple way you can pass list items as parameters to scripts from an automations. The following automation calls a script, and passes a bunch of entity IDs as variable/parameters. Inside the script code, it retrieves the passed entity ids, and uses them to turn the lights off."
comments: true
header: no
image:
    thumb: homeassistant.png
    homepage: header_unsplash_9.jpg
categories:
    - scripts
    - automations
mediaplayer: false
permalink: "/blog/pass-list-to-scripts/"
---

<center><img src="{{site.url}}/images/homeassistant.png"></center>

The code also has a file notify component, where it dumps the information to the log. 

```
homeassistant:

notify:
  - name: file_notify
    platform: file
    filename: debug.log

automation:
  - alias: 'Pass list to script'
    trigger:
      - platform: state
        entity_id: switch.kitchen_light
    action:
      - service: script.test_script
        data:
          message:
            entities_list:
              - light.hue_color_lamp_1
              - light.hue_color_lamp_2
              - light.hue_color_lamp_3
```

The above automation calls the script by passing parameters. Below is the actual script that receives those entity ids.

{% raw %}
```
script:
  test_script:
    sequence:
      - service_template: light.turn_on
        data_template:
          entity_id: >
            {% for e in message.entities_list %}
            {%- if loop.first %}{% elif loop.last %}, {% else %}, {% endif -%}
            {{ e }}
            {%- endfor %}
      - service: notify.file_notify
        data_template:
          message: >
            {% for e in message.entities_list %}
            {%- if loop.first %}{% elif loop.last %}, {% else %}, {% endif -%}
            {{ e }}
            {%- endfor %}
```
{% endraw %}
