---
layout: page-fullwidth
title: "Passing a list as parameters to scripts"
subheadline: "Passing list to Scripts & Automations"
meta_teaser: "Want to pass list items as parameters to the scripts?"
teaser: "Want to pass list items as parameters to the scripts?"
comments: true
header:
    image_fullwidth: header_unsplash_34.jpg
image:
    thumb: bluetooth.jpg
    homepage: header_unsplash_34.jpg
categories:
    - scripts
    - automations
permalink: "/blog/pass-list-2-scripts/"
---

<center><img src="{{site.url}}/images/bluetooth.jpg"></center>

Here is a simple way you can pass list items as parameters to scripts and automations. The following automation calls a script, called `test_script` and it passes a bunch of entity IDs as parameters. 

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
