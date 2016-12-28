---
layout: post
section-type: post
title: Adding a crossbow to terasology
category: tech, programming
tags: [ 'tutorial' ]
---

For the Google Code In, GCI, I picked up a task where I had to create a crossbow for the game [Terasology](https://github.com/MovingBlocks/Terasology).


### Setting up Terasology

Setting up Terasology is really easy:
1. [fork](https://github.com/MovingBlocks/Terasology/fork) their repository
2. {%raw%}cd{%endraw%}  into it
3. run {%raw%}gradlew idea{%endraw%} You can replace {%raw%}idea{%endraw%} with other IDE's, for example eclipse.
4. open the project and develop!


### Creating a new prefab

First of we have to create a new prefab, a file that describes our item.
In the prefab we first define the parent of the item, in most cases it is engine:iconItem.
Ofcourse the items needs a name, here it is "Crossbow" and we need an icon. Icons are defined in an atlas file, the crossbow was already in the items.atlas file so I could use that one.

```JSON
{
  "parent": "engine:iconItem",
  "DisplayName": {
    "name": "CrossBow"
  },
  "Item": {
    "icon": "engine:items#crossbow",
    "usage": "IN_DIRECTION"
  },
  "ArrowAction": {}
}
```

Next is the action that happens when you right-click with the item in hand. Here I called it ArrowAction and you also need a Java-class named like that in the org.terasology.logic.actions package.
The action needs to extend the BaseComponentSystem. Your class needs the function onActivate, this is the actual function should contain the code that you want to execute when the right-mouse button is clicked.

Here is the onActivate function of the crossbow:
```Java
	@ReceiveEvent
  public void onActivate(ActivateEvent event, EntityRef entity, ArrowActionComponent arrowActionComponent) {
      if (time.getGameTime() > lastTime + 1.0f/arrowActionComponent.arrowsPerSecond) {
          Vector3f target = event.getHitNormal();
          Vector3i blockPos = new Vector3i(target);
          Vector3f position = new Vector3f(event.getOrigin());
          Vector3f dir = new Vector3f(event.getDirection());

          HitResult result;
          result = physicsRenderer.rayTrace(position, dir, arrowActionComponent.maxDistance, filter);

          Block currentBlock = worldProvider.getBlock(blockPos);

          if (currentBlock.isDestructible()) {
              EntityBuilder builder = entityManager.newBuilder("engine:defaultBlockParticles");
              builder.getComponent(LocationComponent.class).setWorldPosition(target);
              builder.build();
          }
          EntityRef blockEntity = result.getEntity();
          blockEntity.send(new DoDamageEvent(arrowActionComponent.damageAmount, arrowActionComponent.damageType));
          lastTime = time.getGameTime();
      }
  }
```

This might seem like a lot of magic, but is really is just raytracing and doind damage to a block that is in range.
To see the entire source code, head over to the [Terasology](https://github.com/MovingBlocks/Terasology) and browse around in the modules/Core.