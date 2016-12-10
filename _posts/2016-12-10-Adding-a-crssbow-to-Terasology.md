---
layout: post
section-type: post
title: Adding a crossbow to terasology
category: tech
tags: [ 'tutorial' ]
---

For the Google Code In, GCI, I picked up a task where I had to create a crossbow for the game [Terasology](https://github.com/MovingBlocks/Terasology).

---
Setting up Terasology
---
Setting up Terasology is really easy:
- [fork](https://github.com/MovingBlocks/Terasology/fork) their repository
- {%raw%}cd{%endraw%}  into it
- run {%raw%}gradlew idea{%endraw%} You can replace {%raw%}idea{%endraw%} with other IDE's, for example eclipse.
- open the project and develop!

---
Creating a new prefab
---
First of we have to create a new prefab, a file that describes our item.
