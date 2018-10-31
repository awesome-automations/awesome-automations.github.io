---
layout: page-fullwidth
title: "Iterate through entities"
subheadline: "entities?"
meta_teaser: "How to iteate entities ..."
teaser: "Here is how you can iterate through tntities."
comments: true
header:
  image_fullwidth: header_unsplash_16.jpg
image:
    thumb: ups.jpg
    homepage: header_unsplash_16.jpg
categories:
    - homeassistant
permalink: "/jinja/entities/"    
---

Here is how you can retrieve all the entities and the corresponding attributes using the template editor.

```
{% raw %}
{{ "_".ljust(90, "_") }}
{{ "Entity ID".ljust(50) }}{{ "Entity Name" }}
  {{ "Attribute Name".ljust(50) }}{{ "Attribute Value" }}

{% for item in states %}
{{ "_".ljust(90, "_") }}
{{ item.entity_id.ljust(50) }}
  {{ "State".ljust(50) }}: {{ item.state}}
  {{ "Domain".ljust(50) }}: {{ item.domain}}
  {{ "Object ID".ljust(50) }}: {{ item.object_id}}
  {{ "Last Updated".ljust(50) }}: {{ item.last_updated}}
  {{ "Last Changed".ljust(50) }}: {{ item.last_changed}}
{%- for attrib in item.attributes|sort() %}
{%- if attrib is defined %} 
  {{attrib.ljust(50)}}: {{ item.attributes[attrib] }} 
{%- endif %}
{%- endfor %}
{%- endfor %}
{% endraw %}
```

That script above iterates through the object model, and prints the details as below:

```
__________________________________________________________________________________________
automation.alert_low_battery_level_of_sensors     
  State                                             : on
  Domain                                            : automation
  Object ID                                         : alert_low_battery_level_of_sensors
  Last Updated                                      : 2017-09-14 00:19:00.697024+00:00
  Last Changed                                      : 2017-09-14 00:19:00.697024+00:00 
  friendly_name                                     : Alert Low Battery Level of Sensors 
  icon                                              : mdi:arrow-right-drop-circle 
  last_triggered                                    : None
__________________________________________________________________________________________
automation.alert_super_heavy_winds                
  State                                             : on
  Domain                                            : automation
  Object ID                                         : alert_super_heavy_winds
  Last Updated                                      : 2017-09-14 00:19:00.739659+00:00
  Last Changed                                      : 2017-09-14 00:19:00.739659+00:00 
  friendly_name                                     : Alert Super Heavy Winds 
  hidden                                            : True 
  icon                                              : mdi:arrow-right-drop-circle 
  last_triggered                                    : None

  ...more entries!
  ```
  