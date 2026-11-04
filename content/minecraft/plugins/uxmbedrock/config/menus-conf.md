---
title: menus.conf
order: 603
description: Which menus of other plugins open as forms.
icon: layout-list
---

| Key | Default | Meaning |
|---|---|---|
| `default` | `"form"` | How a menu opens when no rule speaks for it: `form` or `chest` |
| `touch` | `"form"` | `form`: a menu whose title carries a picture opens as a form for touch players, even where a rule says chest. `chest`: they get the plain chest |
| `rules` | `[]` | The rules, below |

## rules

```hocon
rules = [
  { plugin = "ShopGUIPlus", title-contains = "", mode = "form" },
  { plugin = "*", title-contains = "Crate", mode = "chest" },
]
```

| Field | Meaning |
|---|---|
| `plugin` | The plugin's name as `/plugins` shows it, ignoring case; `*` for every plugin |
| `title-contains` | Text the menu's title must hold, ignoring colours and case; empty for any title |
| `mode` | `form` or `chest`. A rule with another word is skipped |

The most specific rule wins: plugin and title, then plugin, then `*`. A player who chose chests always gets chests.
`/uxmbedrock menus` lists the rules in force.
