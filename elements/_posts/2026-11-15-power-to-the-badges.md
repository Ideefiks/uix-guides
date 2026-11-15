---
title: Power to the badges
description: How to use forge and sparks to modify all three elements of a heading badge
excerpt_image: /assets/elements/2026-11-1-badges.png
tags: 
  - badge
  - forge
  - sparks
---
The heading badge only has three elements but can show key information in very little space. As standard however it allows very little customisation, we can override `name`, set `icon` and choose the icon `color`. We cannot change the state. In this guide we will show how to add templating capability to all three elements (icon, name and state).

The badges below show the current temperature as reported by a weather service, the first one is a standard badge.

{% include admonition.html type="homeassistant" title="Standard and modified heading badges" body="![Standard and modified weather badges](/assets/elements/2026-11-15-badges.png)" %}

UIX can forge `name` and `icon`, but not the state because the badge has no entry key for the state. We can get around this limitation by using the search spark, which can look for a specific text and change it into a different text (which can be a template). For this to work the state needs to be a fixed string so the search spark knows what to look for. The only way to achieve this is by setting `state_content: name` and setting this to a fixed string `name: LookForThis`. Take a look at the second badge to see what this achieves.

