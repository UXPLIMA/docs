---
title: Languages and menus
order: 56
description: Messages, ranks, conversions and the menu files.
icon: languages
---

Everything a player reads lives under `languages/<code>/`. Two languages ship (`en` and `tr`) and
`config.yml → language` picks one.

```
languages/en/
├── messages.yml       every chat message
├── ranks.yml          farmer ranks and their items
├── conversions.yml    the conversions menu
└── menus/             one file per menu
```

## Adding a language

Copy `en/` to a new folder, translate it, set `language` to the folder name and reload. Nothing else
references the folder, so a language you add survives plugin updates as long as you keep the keys.

Update it after a plugin update: new messages are written to the shipped languages, not to yours,
and a key your file is missing shows up as empty text.

## messages.yml

MiniMessage throughout. `prefix` is defined at the top and every message can reference it as
`<prefix>`:

```yaml
prefix: "<#ff7700><bold>Farmer <dark_gray>►<reset>"
reloaded: "<prefix> <gray>Configuration files updated."
```

Message-specific placeholders are written in angle brackets (`<player>`, `<amount>`,
`<setting>`) and are filled in by the plugin. PlaceholderAPI placeholders work too, in the
`<papi:'...'>` form.

## Menus

Each menu is a file in `menus/`:

| File | Menu |
|---|---|
| `farmer-main-menu.yml` | The dashboard, with every product |
| `farmer-manage-menu.yml` | Manage: members, transfer, deletion |
| `farmer-settings-menu.yml` | The module switches |
| `farmer-storage-status-menu.yml` | Storage overview |
| `farmer-logs-menu.yml` | The farmer's log |
| `farmer-appearance-settings.yml` | Appearance |
| `farmer-entity-menu.yml` | Entity type |
| `farmer-villager-profession-menu.yml` | Villager profession |
| `glowing-settings-menu.yml` | Glow colour |
| `auto-kill-settings-menu.yml` | Auto Kill's mob list |
| `auto-sell-settings-menu.yml` | Auto Sell's product list |
| `member-list-menu.yml` | Members |
| `member-manage-menu.yml` | One member's rank and removal |
| `member-name-input-menu.yml` | Adding a member |
| `permission-list-menu.yml` | Rank permissions |
| `product-actions-menu.yml` | One product: sell, take, toggle |
| `product-conversions-menu.yml` | Conversions for one product |
| `upgrade-menu.yml` | One product's level ladder |
| `upgrade-all-menu.yml` | Upgrade everything |
| `shop-menu.yml` | Selling |
| `confirm-collecting-menu.yml` | Confirmation before taking all |
| `confirm-transfer-menu.yml` | Confirmation before transferring |

## A menu file

```yaml
title: "<black><bold>Farmer Dashboard"
rows: 6
pageable-slot-size: 28
filler-material: "GRAY_STAINED_GLASS_PANE"
filler-slots: [0, 1, 2, ...]

collected-item:
  material: STONE
  model-data: 0
  amount: 1
  display-name: ""
  glowing: false
  head-data: null
  lore:
    - " <dark_gray>► <gray>Current Stock: <#05e300><amount><dark_gray>/<#e3000e><capacity>"
    - " <dark_gray>► <gray>Unit Value: <#FFD700><unit_price>$"
```

| Key | Meaning |
|---|---|
| `title` | MiniMessage title |
| `rows` | Inventory height, 1 to 6 |
| `pageable-slot-size` | How many slots hold the paged content |
| `filler-material` | The background item |
| `filler-slots` | Which slots it occupies |

Items are defined the same way everywhere in this plugin: `material`, `model-data`, `amount`,
`glowing`, `head-data` and a MiniMessage `display-name` and `lore`. `head-data` takes a texture
value prefixed with `texture-`, or a player name.

`collected-item` is a template: the material is replaced with the actual product, which is why the
comment says to leave `STONE` alone. Its lore is where product placeholders like `<amount>`,
`<capacity>`, `<fullness>`, `<unit_price>` and `<tax>` are available.

<Callout type="tip" title="Slots are zero-indexed">

Slot 0 is the top-left of the inventory and slot 53 the bottom-right of a six-row menu. A slot
number beyond `rows * 9 - 1` is simply never drawn, which is the usual reason an item you added
does not appear.

</Callout>
