---
title: Messages
order: 608
description: How a line is written, the prefixes, the blocks of keys, and the content words.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). English is turned into small capitals when it is drawn, so the file stays
readable. A key that is missing falls back to the default the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'ENCHANTS'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<h:'...'>` | A header |

| Block | Said when |
|---|---|
| `admin.*` | `version`, `usage`, `doctor.*`, `reloaded`, `reload-failed`, `content-broken` |
| `error.*` | `hold-an-item`, `unknown-player`, `unknown-world` |
| `enchant.*` | `unknown`, `applied`, `removed`, `not-on-item`, and the refusals: `not-permitted`, `wrong-item`, `conflicts`, `requires`, `over-maximum`, `limit-reached`, `type-limit-reached` |
| `book.*` | `hint` (a book's lore), `given`, `none-matched`, `inventory-full` |
| `drop.*` | Applying a book by clicking: `applied`, `refused`, `too-expensive` |
| `editor.*` | `bad-id`, `created`, `exists`, `deleted`, `off` |
| `limit.*` | `/enchants limit`: `header`, `base`, `node`, `no-node`, `used`, `left` |
| `list.*`, `export.*`, `descriptions.*`, `strip.*`, `import.*` | Those commands |
| `language.*` | `/enchants lang` and the operator's two |
| `command.*` | The command layer: no permission, player only, invalid values, help |
| `menu.*` | Every window's titles, tiles, buttons, the editor's labels and `group.*` |

## Content words

The shipped content files name their words with `@content.<file>.<path>`, and each messages file carries them:

```
content.enchantments.sharpened.name = "Sharpened"
content.enchantments.sharpened.description = "Cuts deeper with every level."
content.rarities.rarities.common.name = "Common"
content.types.types.curse.name = "Curse"
content.targets.targets.armor.name = "Armour"
content.vanilla.vanilla.mending.name = "Mending"
```

An enchantment of your own may use this form, write one line per language in its file, or write one line for all.

Related: [Languages](languages.md), [theme.conf](theme.md)
