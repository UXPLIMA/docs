---
title: emojis.conf
order: 507
description: The per-line limit and the keys of an emoji.
icon: smile
---

`modules/emojis/emojis.conf`, read at start and on every reload.

```hocon
per-line = 10

emojis {
  heart { glyph = "<red>❤</red>", shortcuts = [ ":heart:" ], permission = "" }
  snowman { glyph = "<white>☃</white>", shortcuts = [ ":snowman:" ], permission = "uxmchat.emoji.seasonal" }
}
```

| Key | Meaning |
|---|---|
| `per-line` | The most emoji one line may turn into glyphs |
| `glyph` | MiniMessage, so it can carry a colour; characters the game's font or a resource pack draws |
| `shortcuts` | What a player types; a word in colons is the usual form |
| `permission` | Needed to use it; empty lets everyone |

The eighteen shipped emoji are listed in [Emoji](../features/emojis.md).

Related: [Emoji](../features/emojis.md)
