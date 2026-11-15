---
title: Power to the badges
description: How to use forge and sparks to modify all three elements of a heading badge
excerpt_image: /assets/elements/2026-11-1-badges.png
tags: 
  - badge
  - forge
  - sparks
---
The heading badge only has three elements but can show key information in very little space. As standard however it allows little customisation, we can override `name`, set `icon` and choose the icon `color`. We cannot change the state and we cannot use templates. In this guide we will show how to add templating capability to all three elements (icon, name and state).

The badges below show the current temperature as reported by a weather service, the first one is a standard badge.

{% include admonition.html type="homeassistant" title="Standard and modified heading badges" body="![Standard and modified weather badges](/assets/elements/2026-11-15-badges.png)" %}

UIX can forge `name` and `icon` and add template capability to both, but not the state because the badge has no entry key for the state. We can get around this limitation by using the search spark, which can look for a specific text and change it into a different text (which can be a template). For this to work the state needs to be a fixed string so the search spark knows what to look for. The only way to achieve this is by setting `state_content: name` and setting this to a fixed string `name: LookForThis`. Take a look at the second badge to see what this achieves.

Now we can add two sparks to change both the state and the name to something else (for this specific example forging the name and icon would have been possible without the sparks but by using them we can also catch unavailability of the state).

{% raw %}

```
type: custom:uix-forge
forge:
  mold: badge
  sparks:
    - type: search
      for: hui-entity-badge $
      query: state-display
      actions:
        replace_text:
          find: name
          replace: >
            {% if not states('sensor.tulpenlaan_current_temperature') in
            ['unavailable', 'unknown'] %}
              {{ states('sensor.tulpenlaan_current_temperature', with_unit=true) }}
            {% else %}
              {{ ' °C' }}      
            {% endif %}
    - type: search
      for: hui-entity-badge $ ha-badge $
      query: span.label
      actions:
        replace_text:
          find: name
          replace: >
            {% set warnings =
            state_attr('binary_sensor.weather_warning_tulpenlaan',
              'active_warnings_friendly_names') | lower %}
            {% if warnings == "" %}  {{ 'KMI' }}   {% else %}    {{
            warnings[0:9] }} {% endif %}
element:
  type: entity
  entity: sensor.tulpenlaan_current_temperature
  state_content: name
  show_name: true
  name: name
  icon: |-
    {%
     set condition = state_attr('sensor.tulpenlaan_forecast_hourly', 'forecast') | selectattr('datetime', '>', (now() - timedelta(hours=0.5)).isoformat()) | 
    selectattr('datetime', '<=',  (now() + timedelta(hours=0.5)).isoformat()) | 
    map(attribute='condition') | list %}
    {%
     set day = state_attr('sensor.tulpenlaan_forecast_hourly', 'forecast') | selectattr('datetime', '>', now().isoformat()) | 
    selectattr('datetime', '<=',  (now() + timedelta(hours=1)).isoformat()) | 
    map(attribute='is_daytime') | list %}
    {% if condition == ['cloudy'] %}
    {{ 'local:material-symbols-light--cloud-outline-rounded' }}
    {% elif condition == ['partlycloudy'] and day == [true] %}
    {{ 'local:material-symbols-light--partly-cloudy-day-outline-rounded' }}
    {% elif condition == ['partlycloudy'] and day == [false] %}
    {{ 'local:material-symbols-light--partly-cloudy-night-outline-rounded' }}
    {% elif condition == ['sunny'] %}
    {{ 'local:material-symbols-light--wb-sunny-outline-rounded' }}
    {% elif condition == ['partly-rainy'] %}
    {{ 'local:material-symbols-light--rainy-light-outline-rounded' }}
    {% elif condition == ['rainy'] %}
    {{ 'local:material-symbols-light--rainy-outline-rounded' }}
    {% elif condition == ['pouring'] %}
    {{ 'local:material-symbols-light--rainy-heavy-outline-rounded' }}
    {% elif condition == ['clear-night'] %}
    {{ 'local:material-symbols-light--moon-stars-outline-rounded' }}
    {% elif condition == ['partly-snowy'] %}
    {{ 'local:material-symbols-light--sunny-snowing-outline-rounded' }}
    {% elif condition == ['snowy'] %}
    {{ 'local:material-symbols-light--weather-snowy-outline-rounded' }}
    {% elif condition == ['fog'] %}
    {{ 'local:material-symbols-light--mist-outline-rounded' }}
    {% elif condition == ['hail'] %}
    {{ 'local:material-symbols-light--weather-hail-outline-rounded' }}
    {% elif condition == ['lightning'] %}
    {{ 'local:material-symbols-light--thunderstorm-outline-rounded' }}
    {% elif condition == ['lightning-rainy'] %}
    {{ 'local:material-symbols-light--thunderstorm-outline-rounded' }}
    {% elif condition == ['snowy-rainy'] %}
    {{ 'local:material-symbols-light--weather-snowy-outline-rounded' }}
    {% elif condition == ['windy'] %}
    {{ 'local:material-symbols-light--air-outline-rounded' }}
    {% elif condition == ['windy-variant'] %}
    {{ 'local:material-symbols-light--air-outline-rounded' }}
    {% elif condition == ['exceptional'] %}
    {{ 'local:material-symbols-light--cyclone-outline-rounded' }}
    {% else %}
    {{ 'local:material-symbols-light--partly-cloudy-day-outline-rounded' }}
    {% endif %}
  tap_action:
    action: more-info
    entity: weather.tulpenlaan
  double_tap_action:
    action: more-info
    entity: camera.radar_tulpenlaan
```
