---
title: tags.conf
order: 609
description: The wrapper, surfaces, effects, import, categories, rarities, player-made rules and every key of a tag.
icon: tag
---

`modules/tags/tags.conf`. Everything but `tags` is read on every reload; the `tags` block is read once, into the
database, and again only if the tags table is empty.

| Block | Holds |
|---|---|
| `wrapper` | `"<tag> "`: how a worn tag sits before the name |
| `surfaces` | `tab-list = true`, `display-name = false` |
| `effects` | `set`, `cleared`, `lapsed` |
| `import.only` | Ids to bring over from uxm-tags; empty imports all |
| `categories` | `display`, `icon`, `order`, `permission` |
| `rarities` | `common`, `rare`, `epic`, `legendary`: the word shown on a tag's button |
| `player-made` | `approval`, `max-length`, `per-player`, `category` |
| `tags` | The tags a new database starts with |

## A tag

| Key | Meaning |
|---|---|
| `display` | MiniMessage, or `@key` for a line of the messages files |
| `frames` | For an animated tag: the displays shown in turn |
| `frame-millis` | The time between frames |
| `category`, `icon`, `order`, `rarity` | Where and how it sits in the window |
| `permission` | Who holds it; left out it is `uxmchat.tag.<id>`, or buying it when it has a price. `""` with no price and no groups gives it to every player |
| `groups` | Permission groups that hold it without the permission |
| `default-priority` | A player who chose nothing wears the held tag with the highest priority |
| `hide-when-locked` | Hide it in the window from players who do not hold it |
| `variants` | Other looks of the tag, each with its own `display` and `permission` |
| `price` | `currency`, `amount`, `conditions`, `on-success`, `on-failure` |

```hocon
star {
  display = "<#FFE66D>[★]"
  frames = [ "<#FFE66D>[★]", "<#FFA07A>[★]", "<#FF6B8B>[★]", "<#FFA07A>[★]" ]
  frame-millis = 500
  category = "events"
  icon = "NETHER_STAR"
  rarity = "legendary"
  hide-when-locked = true
}
```

Related: [Tags](../features/tags.md)
