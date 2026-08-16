---
title: Menu Customization
order: 790
description: Every GUI in uxmSkyblock is defined in a YAML file under plugins/uxmSkyblock/menus/.
  You can move buttons, swap icons, reword text, and re-theme the whole plugin without
  code.
icon: layout-grid
---

---

## The Menu Files

| File | Menu |
|------|------|
| `main.yml` | Main menu (`/is`): see [Main Menu](../menus/main-menu.md) |
| `settings.yml` | Settings / flags, see [Settings](../menus/settings.md) |
| `upgrades.yml` | Upgrades, see [Upgrades](../menus/upgrades.md) |
| `warp.yml` | Public warps, see [Warps](../menus/warps.md) |
| `blocks.yml` | Block values (`/is block`): see [Block Values](../menus/blocks.md) |
| `create.yml` | Island type picker (`/is create`): see [Island Type](../menus/island-type.md) |
| `help.yml` | Command/help list |
| `delete-confirm.yml` | Delete confirmation |

---

## Anatomy of a Menu

```yaml
title: "&8Island Menu"
rows: 5
type: warp            # plain | warp | upgrades | blocks

items:
  home:
    slot: 37
    material: GRASS_BLOCK
    amount: 1
    custom-model-data: 1001   # optional
    name: "&aGo To My Island"
    lore:
      - "&eClick to teleport!"
    action: "island:git"
```

| Field | Meaning |
|-------|---------|
| `title` | Inventory title (color codes allowed) |
| `rows` | 1–6 rows |
| `type` | Special behavior (see below) |
| `items.<id>.slot` | Slot index (0-based) |
| `items.<id>.material` | Item material |
| `items.<id>.amount` | Stack size (default `1`) |
| `items.<id>.custom-model-data` | Optional, see [Custom Model Data](#custom-model-data) |
| `items.<id>.name` / `lore` | Display text |
| `items.<id>.action` | What clicking does |

---

## Custom Model Data

Any menu item may add a `custom-model-data` line to render a **resource pack /
ItemsAdder / Oraxen** model instead of the vanilla texture:

```yaml
items:
  warps:
    slot: 4
    material: PAPER
    custom-model-data: 1001
    name: "&bWarps"
    action: "open:warp"
```

- The value is the `CustomModelData` number your resource pack maps to a model.
- Omit the line (or set it to `-1`) to keep the default vanilla look.
- Works on **every** item in **every** menu file under `plugins/uxmSkyblock/menus/`.

<Callout type="tip">

Reload with `/is admin reload` after editing.

</Callout>

---

## Menu Types

| `type` | Behavior |
|--------|----------|
| *(plain)* | Just the items you define |
| `warp` | Center slots auto-fill with online islands' heads; clicking visits them |
| `upgrades` | Items auto-fill from [upgrades.yml](upgrades-yml.md) |
| `blocks` | Items auto-fill from [block-values.yml](block-values-yml.md), paginated |
| `schematics` | Items auto-fill from the `schematics:` list; clicking creates that island type, paginated |

---

## Actions

| Action | Effect |
|--------|--------|
| `open:<menu>` | Open another menu (e.g. `open:warp`) |
| `island:<sub>` | Run an island sub-command (e.g. `island:git`, `island:olustur`) |
| `flag:<NAME>` | Toggle an island flag (e.g. `flag:PVP`) |
| `page:prev` / `page:next` | Page navigation |
| `close` | Close the menu |

`island:<sub>` uses the **internal** sub-command name (see
`command.subcommands` in [config.yml](config-yml.md)).

---

## Placeholders in Menus

Menu text supports the plugin's own placeholders (`%skyblock_level%`,
`%skyblock_flag_pvp%`, `{owner}`, `{level}`, `{points}`, `{member_list}`, ...) and,
when installed, any **PlaceholderAPI** placeholder (e.g. `%vault_group%`). The
`%skyblock_...%` ones render even **without** PlaceholderAPI.

<Callout type="tip">

Reload menus with `/is admin reload` after editing.

</Callout>
