---
title: Power to the badges
description: How to use forge and sparks to modify all three elements of a heading badge, as well as theme and card level styling
excerpt_image: /assets/elements/2026-11-1-badges.png
tags: 
  - badge
  - forge
  - sparks
  - themes
---
The heading badge only has three elements but can show key information in very little space. As standard however it allows very little customisation, we can override the name, choose an icon and choose the icon color. We cannot change the state. In this guide we will show how to add templating capability to all three elements (icon, name and state) and give an example on how to style it, using a combination of themes and card level styling for icon animations.

UIX can only forge the name, not the state, because the badge only has an entry key to override the name. We can get around this limitation by using the search spark, which can look for a specific text and change it into a different text (which can be a template). For this to work the state needs to have a fixed string so the search spark knows what to look for. The only way to achieve this is by setting `state_content: name` and overriding `name: LookForThis`. Take a look at the second badge to see what this achieves

Our goal is to create a badge that gives info from your favourite weather service, a standard badge showing the current temperature would show something like below. Our goal is dynamically change and style the name (we can change this in the UI but not using a template), and change the icon and its animation based on the current weather situation.
{% include admonition.html type="homeassistant" title="Standard and modified heading badges" body="![Standard and modified weather badges](/assets/elements/2026-11-14-badges.png)" %}

