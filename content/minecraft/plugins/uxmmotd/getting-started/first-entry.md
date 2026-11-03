---
title: Your first entry
order: 204
description: Replace the shipped entries with your network's, add an icon, and check it in a client.
icon: flag
---

1. Open `config.conf` and replace `UXM Network` and `play.uxm.net` in both entries with your own name and address.
   Keep `<plain>` around the address so it stays in ordinary letters.
2. Make a 64 by 64 PNG of your logo, put it in `icons/logo.png`, and set `icons.normal = "icons/logo.png"`. Remove
   the `icon` lines of the entries, or point them at your own pictures.
3. Change the `hover` lines: your gamemodes, your Discord, your store.
4. `/uxmmotd reload`.
5. Refresh the multiplayer list in a client. Refresh again: with `rotation = "random"` the entries take turns.

```hocon
motd {
  rotation = "in-order"
  entries = [
    {
      line-1 = "<center><b><g:'Mesa Craft':wheel></b> <icon>• <value>1.21 - 26.3"
      line-2 = "<center><good>Survival season 3 <icon>• <accent><plain>play.mesacraft.net</plain>"
    }
  ]
}
```

Related: [Entries](../features/entries.md), [Icons](../features/icons.md)
