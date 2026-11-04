---
title: Your first pack
order: 204
description: From a fresh install to a Bedrock player with the server's own items and menus.
icon: package
---

1. Install as [Installation](installation.md) says and start the server. The log says `The Bedrock pack is built`
   with the counts of items, blocks, sounds and glyphs.
2. Run `/uxmbedrock pack report`. Its first line counts what was converted; the `Read:` lines name every source it
   found. Your item plugin's pack is there when the plugin wrote it before the build; run `/uxmbedrock pack build`
   after the item plugin finished writing.
3. Run `/uxmbedrock items unknown`. An item model listed there has no vanilla item under it that the plugin could
   learn; give it in [items.conf](../config/items-conf.md) and build again. A model is learned on its own the first
   time any player holds an item with it.
4. Restart Geyser (or the server). The log of the extension says how many custom items it registered.
5. Join with Bedrock. Your items show their icons, a chest menu of a shop opens as a form, and `/bedrock` opens your
   settings.
6. When a menu background sits wrong on Bedrock, open that menu after `/uxmbedrock gui measure`, or place it in the
   [preview page](../features/preview.md).
