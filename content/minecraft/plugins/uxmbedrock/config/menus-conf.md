---
title: menus.conf
order: 603
description: Which menus of other plugins open as forms.
icon: layout-list
---

| Key | Default | Meaning |
|---|---|---|
| `default` | `"form"` | How a menu opens when no rule speaks for it: `form` or `chest` |
| `touch` | `"chest"` | For players who steer by touch, a menu whose title carries a picture. `chest`: the chest with its picture, laid out as on a PC on phones and tablets too. `form`: a form, easier to tap, even where a rule says chest |
| `rules` | `[]` | The rules, below |

## rules

```hocon
rules = [
  { plugin = "ShopGUIPlus", title-contains = "", mode = "form" },
  { plugin = "*", title-contains = "Crate", mode = "chest" },
  { plugin = "*", title-contains = "Settings", mode = "settings" },
]
```

| Field | Meaning |
|---|---|
| `plugin` | The plugin's name as `/plugins` shows it, ignoring case; `*` for every plugin |
| `title-contains` | Text the menu's title must hold, ignoring colours and case; empty for any title |
| `mode` | `form`, `chest` or `settings`. A rule with another word is skipped |

The most specific rule wins: plugin and title, then plugin, then `*`. A player who chose chests always gets chests.
`/uxmbedrock menus` lists the rules in force. `/uxmbedrock menus report` lists the menus Bedrock players opened since
the start, how each reached them and whether its title carries a picture, and writes them to
`plugins/uxmBedrock/reports/menus.json`, so a menu that needs a rule is easy to find.
