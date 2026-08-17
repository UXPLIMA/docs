---
title: Menu Files
order: 42
icon: file-cog
---

Menus live under `languages/<language>/menus/`, one YAML file per menu, so a menu is translated by
translating a file rather than by editing a shared one.

| File | Menu | Opened by |
|---|---|---|
| `npc-main-menu.yml` | The NPC's main menu | `[menu] main` |
| `npc-modify-menu.yml` | Appearance | `[menu] appearance` |
| `glowing-settings-menu.yml` | Glow colours | `[menu] glow` |
| `shop-menu.yml` | Purchase | `[menu] shop`, or `/helpernpc buy` |

## Shape of a menu file

```yaml
title: "              <black>Island Info"
rows: 3
filler-material: "GRAY_STAINED_GLASS_PANE"
filler-slots: [0, 1, 2, 3, 5, 6, 7, 8]

items:
  info:
    material: "PLAYER_HEAD"
    model-data: 0
    amount: 1
    display-name: "<#ff0000>Island Info"
    glowing: false
    head-data: "texture-ewogICJ0aW1lc3RhbXAi..."
    lore:
      - '<#00b4d8>Island Profile'
      - ' <#DCF7F7>Level <#81e0f7>%superior_island_level_format%'
    slots: [4]
    actions:
      - "[player] is"
      - "[close]"
```

| Key | Meaning |
|---|---|
| `title` | Menu title, MiniMessage |
| `rows` | Inventory rows |
| `filler-material` | Material for the empty slots |
| `filler-slots` | Which slots the filler occupies |
| `items` | A map of named items — the name is yours, only `actions` and `slots` matter to the plugin |

## Item keys

| Key | Meaning |
|---|---|
| `material` | Bukkit material name |
| `model-data` | Custom model data, for resource packs |
| `amount` | Stack size |
| `display-name` | MiniMessage, with PlaceholderAPI placeholders |
| `glowing` | Enchantment glint |
| `head-data` | `texture-<base64>` for a player head, or a player name |
| `lore` | Lines, MiniMessage, with placeholders |
| `slots` | Every slot this item is placed in |
| `actions` | Run in order when clicked, see [Actions](actions.md) |

An item may sit in several slots; the same item at slots `[3, 5]` is one definition, not two.

<Callout type="tip" title="Placeholders come from your other plugins">

The shipped main menu is full of `%superior_island_*%` placeholders because it was written against
SuperiorSkyblock2. Replace them with the placeholders of whichever region plugin you run, or the
lines will render as raw text.

</Callout>
