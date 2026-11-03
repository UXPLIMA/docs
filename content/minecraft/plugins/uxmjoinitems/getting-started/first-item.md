---
title: Your first item
order: 204
description: A profile item in slot 2 that runs a command and plays a sound.
icon: flag
---

Create `items/profile.conf`:

```hocon
material = "PLAYER_HEAD"
slot = 2
name = "<accent>Your profile"
lore = [ "<subtext>Right click to see your stats" ]
glow = false
unbreakable = true
triggers = ["join", "respawn"]
cooldown = "1s"
restrictions {
  locked = true
}
right-click = [
  "[menu] stats"
  "[sound] minecraft:ui.button.click 1 1.2"
]
```

1. `/uxmjoinitems reload`.
2. `/uxmjoinitems give <you> profile` to see it now, or rejoin.
3. Right click it: the open screen closes and `/stats` runs as you.

To move a player to another server instead, write `"[server] survival"`.

Related: [Item files](../config/item-files.md), [Clicks](../features/clicks.md)
