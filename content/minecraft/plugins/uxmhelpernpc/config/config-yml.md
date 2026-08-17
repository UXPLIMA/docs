---
title: config.yml
order: 52
icon: file-cog
---

## hooks

```yaml
hooks:
  region-hook: "uxmClaims"
  money-hook: "Vault"
  interaction-hook: "FancyNpcs"
```

The three plugins uxmHelperNPC leans on. See [Integrations](../integrations.md) for every accepted
value and the notes that come with them.

## database

```yaml
database:
  driver: "SQLITE"     # SQLITE or MYSQL
  host: "localhost"
  database: "database"
  username: ""
  password: ""
  port: 3306
  use-ssl: false
```

See [Database](../database.md).

## multi-server

```yaml
multi-server:
  enabled: false
  redis-host: ""
```

Turn this on when more than one server shares the same database, so NPC changes made on one are
seen by the others.

## settings

| Option | Default | What it does |
|---|---|---|
| `disabled-worlds` | `["worlds"]` | Worlds where NPCs and their commands are refused |
| `price` | `15000.0` | What an NPC costs through the shop menu |
| `return-price-on-delete` | `true` | Whether deleting refunds anything |
| `return-percent` | `75` | Share of the price refunded by `/helpernpc return` |
| `maximum-npc-per-player` | `2` | Cap per player |
| `place-npc-on-region-creation` | `true` | Hand a player their NPC when their region is created |
| `helpernpc-command-opens-gui` | `false` | Make bare `/helpernpc` open the menu instead of printing help |
| `helpernpc-command-opens-buy-menu` | `true` | With the above on, open the shop when the player has no NPC |
| `disable-buy-command` | `false` | Turn off `/helpernpc buy` so eggs are the only route |
| `default-click-actions` | menu + sound | What clicking the NPC body runs |

```yaml
first-location:
  enabled: false
  modify-by: -5.5, 1.5, 3.5
  rotation: -88, 0.8
```

Where an automatically placed NPC lands, relative to the centre of the region. `modify-by` is an
X, Y, Z offset and `rotation` is pitch and yaw. It only applies when
`place-npc-on-region-creation` is on.

## model-settings

```yaml
model-settings:
  default-entity: VILLAGER
  model-source: ModelEngine
  model-name: "test"
```

Only read when `interaction-hook` is `ModelSystem`. See [Appearance](../features/appearance.md).

## hologram

Covered in [Hologram](../features/hologram.md).

## npc

Covered in [Appearance](../features/appearance.md).

## artificial-intelligence

Covered in [AI Assistant](../features/ai-assistant.md).

## cooldown

```yaml
cooldown:
  return-cooldown: 120
  move-cooldown: 180
  change-glowing-cooldown: 120
  ask-question-cooldown: 15
```

Seconds. Every one of them can be waived per player by `/uxmhelpernpc bypass`, and referenced from
a menu button with `[cooldown] <name>`.

## sounds

```yaml
sounds:
  on-cooldown: BLOCK_ANVIL_LAND
  try-again: BLOCK_NOTE_BLOCK_PLING
  already-using-that-glow-color: UI_BUTTON_CLICK
  blacklisted-world: BLOCK_ANVIL_BREAK
```

Bukkit sound names, played on the four refusals the plugin has.

## Top of the file

```yaml
config-version: "..."   # do not change
language: en            # tr or en
license-key: ""
```

`config-version` is written by the plugin and used to migrate your file between versions. Leave it
alone. `language` picks which folder under `languages/` is loaded, for both messages and menus.
