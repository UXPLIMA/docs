---
title: packages.yml
order: 302
description: Shop categories and the packages on sale.
icon: package
---

Two sections: the categories that make the shop's tabs, and the packages inside them. The file ships
with three categories and no packages, because a package points at a schematic that only exists on your
server.

## categories

```yaml
categories:
  houses:
    display: "<gold>Houses</gold>"
    icon: OAK_DOOR
    slot: 0
    permission: ""
```

| Key | Meaning |
|---|---|
| `display` | The tab's name, MiniMessage |
| `icon` | The tab's material |
| `slot` | Its position among the tabs |
| `permission` | Needed to see the category; empty means everyone |

## packages

```yaml
packages:
  castle_medium:
    display: "<dark_purple>Medium Castle</dark_purple>"
    description:
      - "<gray>Four towers, a courtyard, curtain walls.</gray>"
    category: castles
    schematic: castle_medium.schem
    icon: STONE_BRICKS
    price: 75000
    permission: "uxmbuilders.package.castle"
    cooldown: 3600
    minBuilderLevel: 2
    glow: true
    buildOrder: LAYERED
    builderAffinity:
      mason: 1.15
```

| Key | Default | Meaning |
|---|---|---|
| `display` | the id | Shop name, MiniMessage |
| `description` | none | Lore lines in the shop |
| `category` | | A category id from above |
| `schematic` | | A file in `schematics/` |
| `icon` | `PAPER` | Shop material |
| `price` | `0` | `FLAT` price, or the base of `HYBRID` |
| `permission` | empty | Needed to buy and place; empty means everyone |
| `cooldown` | `0` | Seconds between two starts of this package; the global `start.cooldownSeconds` wins if longer |
| `minBuilderLevel` | `1` | The selected builder must be at least this level |
| `glow` | `false` | Shimmer on the shop icon |
| `buildOrder` | `schematic.buildOrder` | Overrides the global order for this package |
| `builderAffinity` | none | Speed multiplier per builder id |

A player whose builder is below `minBuilderLevel` sees the requirement in the shop and cannot buy, rather
than buying a blueprint the build would then refuse.

## Editing in game

| Command | Does |
|---|---|
| `/builders admin package create <id> <schematic> <price>` | Add a package with seed values |
| `/builders admin package edit <id> <field> <value>` | Change one field |
| `/builders admin package info <id>` | Size, block count and materials |
| `/builders admin package list` | Every package |
| `/builders admin package delete <id>` | Remove one |

`edit` accepts `display`, `schematic`, `price`, `permission`, `category`, `icon`, `description`,
`cooldown`, `minBuilderLevel`, `glow` and `buildOrder`, in any letter case.

## Notes

- Every package and the wildcard `uxmbuilders.package.*` are registered as permissions that operators do
  not get by default, so staff test the same rules players meet.
- Replacing a package's schematic file and reloading refunds the unbuilt share of builds of it that a
  restart catches mid-way, because their saved progress refers to the old file.

Related: [Your first build](../getting-started/first-build.md), [builders.yml](builders-yml.md)
