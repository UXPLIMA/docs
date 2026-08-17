---
title: items.yml
order: 54
description: The egg and the other items the plugin hands out.
icon: package
---

`items.yml` describes the NPC egg — the item that turns into an NPC when a player right-clicks it.

```yaml
npc-egg:
  give-even-if-has-npc: true
  item:
    material: "VILLAGER_SPAWN_EGG"
    model-data: 0
    amount: 1
    glowing: true
    head-data: null
    display-name: "<#edaa0e><bold>Helper Egg"
    lore:
      - " "
      - " <#aeed0e>This egg gives you a helper."
      - " <#797d73><italic>(Right-Click)"
```

| Key | Meaning |
|---|---|
| `give-even-if-has-npc` | Whether `/uxmhelpernpc giveegg` works on a player who already owns an NPC |
| `item` | The same item shape menus use, see [Menu Files](../menus/menu-files.md) |

## How an egg is recognised

Every egg handed out by `/uxmhelpernpc giveegg` is stamped with a unique tag when it is created.
The right-click listener reads the tag, not the material or the name — so a villager spawn egg a
player found in a chest stays a villager spawn egg, and an egg cannot be forged by renaming an
item.

Right-clicking a valid egg places the NPC and consumes exactly one egg. If the placement is
refused — the player is over their limit, or outside their region — the egg is not consumed.

<Callout type="tip" title="Giving eggs when inventories are full">

`giveegg` fills empty slots only. Anything that did not fit is reported to both the admin and the
player, with the count that was dropped from the request, so nobody is left wondering where three
of ten eggs went.

</Callout>
