---
title: packages.yml
order: 402
description: Shop categories and the packages on sale.
icon: package
---

Two sections: the categories that make the shop's tabs, and the packages inside them. The file ships
with three categories and no packages, because a package points at a schematic that only exists on your
server. A commented template at the bottom of the file shows two packages.

## categories

```yaml
categories:
  houses:
    display: "<gold>Houses</gold>"
    icon: OAK_DOOR
    slot: 0
    permission: ""
```

| Key | Default | Meaning |
|---|---|---|
| `display` | the id | The tab's name, MiniMessage |
| `icon` | `PAPER` | The tab's material |
| `slot` | none | Its slot in the shop's top row, `0` to `8`. A tab without one, or whose slot is taken, goes into the next free slot |
| `permission` | empty | Needed to see the category and use its packages; empty means everyone |

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
| `category` | | A category id from above; an unknown one is logged |
| `schematic` | | A file in `schematics/` |
| `icon` | `PAPER` | Shop material |
| `price` | `0` | `FLAT` price, or the base of `HYBRID` |
| `permission` | empty | Needed to buy and place; empty means everyone |
| `cooldown` | `0` | Seconds between two starts of this package by one player, on top of the global `start.cooldownSeconds` |
| `minBuilderLevel` | `1` | The selected builder must be at least this level |
| `glow` | `false` | Shimmer on the shop icon |
| `buildOrder` | `schematic.buildOrder` | Overrides the global order for this package |
| `builderAffinity` | none | Speed multiplier per builder id |

A player whose builder is below `minBuilderLevel` sees the requirement in the shop and cannot buy, rather
than buying a blueprint the build would then refuse. `uxmbuilders.package.<id>` and
`uxmbuilders.package.*` grant a package whatever its `permission` says.

An entry left empty, a package or a category with nothing under it, is skipped with a warning instead of
stopping the file from loading.

## Editing in game

| Command | Does |
|---|---|
| `/builders admin package create <id> <schematic> <price>` | Add a package with seed values |
| `/builders admin package edit <id> <field> <value>` | Change one field |
| `/builders admin package info <id>` | Size, block count and materials |
| `/builders admin package list` | Every package |
| `/builders admin package delete <id>` | Remove one |

`edit` accepts `display`, `schematic`, `price`, `permission`, `category`, `icon`, `description`,
`cooldown`, `minBuilderLevel`, `glow` and `buildOrder`, in any letter case. The value is the rest of the
line; a `description` takes its lines separated by `|`. A value that cannot apply is refused: a category
or schematic that does not exist, a `glow` that is not `true` or `false`, an unknown build order.

Every change is written to `packages.yml` at once, keeping its comments, and applies without a reload.
The package's permission node is registered or removed with it.

## Notes

- Every package and the wildcard `uxmbuilders.package.*` are registered as permissions that operators do
  not get by default, so staff test the same rules players meet.
- If a package's schematic file is replaced, a build a restart catches mid-way carries on when the new
  file gives the same order of blocks, which is usually the case when only materials changed. A
  different shape closes the build and refunds the unbuilt share, because its saved progress refers to
  the old one.

Related: [Your first build](../getting-started/first-build.md), [builders.yml](builders-yml.md)
