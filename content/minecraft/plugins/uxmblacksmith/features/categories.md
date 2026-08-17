---
title: Categories
order: 201
description: Grouping trades, gating them by permission, and capping concurrency.
icon: folder
---

A category is a tab in the category menu and a file of trades behind it. Defined in `config.yml`.

```yaml
categories:
  pickaxes:
    name: "Pickaxes"
    permission: ""
    trades: "pickaxes.yml"
    slot: 11
    hide: false
    useNavigation: true
    sameTimeCraftCount: 0
    menuDisplayItem:
      material: "DIAMOND_PICKAXE"
      displayName: "<gradient:#F8E889:#C97B36><bold>Pickaxes</bold></gradient>"
      lore:
        - ""
        - "<gray>Themed blacksmith contracts.</gray>"
        - ""
      key: "q"
```

| Key | Default | What it does |
|---|---|---|
| `name` | — | Display name |
| `permission` | empty | Node required to see and open it; empty means everyone |
| `trades` | `pickaxes.yml` | The file under `categories/` holding its trades |
| `slot` | `0` | Where its icon sits in the category menu |
| `hide` | `false` | Hide it from the menu entirely |
| `useNavigation` | `true` | Show the navigation row inside it |
| `sameTimeCraftCount` | `0` | Concurrent trades allowed from this category; `0` and `-1` mean unlimited |
| `menuDisplayItem` | — | The icon: material, name, lore, model data, layout key |
| `guiLayout` | unset | An override layout for this category's screen |

## The trade file

`trades:` names a file under `categories/`. If the file does not exist, the plugin **generates** it
with worked defaults and logs `Generated category file: categories/<file>`. That is the intended way
to start a category — declare it, restart, then edit what was written.

Two categories may not share a file.

## Permission gating

```yaml
permission: "uxmblacksmith.category.legendary"
```

Any node you like — it is a config value, not a plugin constant. A player without it does not see the
category. This is the cleanest way to sell access to a tier of recipes without touching individual
trades.

## Concurrency

`sameTimeCraftCount` caps how many trades a player may run *from this category* at once. It is
separate from the global slot count:

| Setting | Limits |
|---|---|
| `uxmblacksmith.slot.<n>` | Total concurrent trades, across everything |
| `category.sameTimeCraftCount` | Concurrent trades from one category |
| `trade.sameTimeCraftCount` | Concurrent copies of one trade |

A player with 5 slots and a category capped at 1 can run five trades, but only one of them from that
category.

## Mastery

The category key is the default **mastery key**. Crafting from `pickaxes` grants mastery in
`pickaxes`, and `%uxmblacksmith_mastery_level_pickaxes%` reads it.

A trade can override this with `masteryKey`, which is how several categories can feed one shared
mastery track.

<Callout type="tip" title="Name category keys the way you want masteries named">

The key becomes a mastery name, a placeholder suffix and a perk `scopeKey`. `pickaxes` is a good key;
`cat1` will haunt you the first time you write a scoreboard.

</Callout>
