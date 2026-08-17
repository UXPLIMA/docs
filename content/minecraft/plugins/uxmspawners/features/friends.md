---
title: Friends
order: 38
description: Letting other players use a spawner without giving them the block.
icon: user-plus
---

The friends module lets a spawner owner give other players access to their spawner.

```yaml
friends:
  enabled: true
  permission: uxmspawners.friends
```

A friend can do everything the owner can — open the menu, withdraw, sell, toggle the modules —
with two exceptions they cannot do: **break the spawner** and **add more friends**. Those stay
with the owner.

The permission is what a player needs in order to *add* friends. Being added does not require any
permission, so a player without it can still be someone else's friend.

<Callout type="note" title="Friends are per spawner, not per player">

The list belongs to the spawner. Moving a spawner or giving it away does not carry the list to a
new owner's other spawners, and breaking it ends the arrangement.

</Callout>
