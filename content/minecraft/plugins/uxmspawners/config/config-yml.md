---
title: config.yml
order: 52
description: Worlds, blocks, mob behaviour, auto-save and the spawner list.
icon: file-cog
---

## Top level

```yaml
license-key: ""
config:
  version: ...
  lang: en          # en, tr, pl, de, fr
database:
  type: SQLITE      # leave as SQLITE
```

`config.version` is written by the plugin — leave it alone. `database.type` currently has one
supported value; do not change it.

## World and block behaviour

| Option | Default | What it does |
|---|---|---|
| `disabled-worlds` | `[testWorld]` | No spawner, vanilla or otherwise, may be placed or broken here |
| `open-gui-on-click` | `true` | Right-clicking a spawner opens its menu |
| `disable-xp-drops` | `false` | Spawner mobs drop no XP on the ground |
| `disable-vanilla-spawners-spawning` | `false` | Vanilla spawners stop producing entirely |
| `protect-spawners-from-explosions` | `false` | Explosions cannot destroy spawners |

## Mob behaviour

```yaml
nerf-spawner-mobs:
  enabled: false
  nerf-every-mob: false
  mobs: [ZOMBIE]

no-equipment:
  enabled: false

use-modern-autokill:
  enabled: true
```

Covered in [Auto-Kill](../features/auto-kill.md).

## Auto-save

```yaml
auto-save:
  interval: 300
  console-log: true
```

Spawner data is written every `interval` seconds. This is the window you lose in a crash — 300 is
the recommended value, and lowering it below about 60 buys little for the write cost. Keep
`console-log: true`: it is where you find out that saving is failing.

## spawners

The spawner type definitions. See [Spawner Types](spawner-types.md).

## itemPrices

Where sell prices come from. See [Prices](prices.md).
