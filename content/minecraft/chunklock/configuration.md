---
title: Configuration
order: 1700
description: 'Chunklock''s settings live in its own config, separate from the core:'
---

```
plugins/uxmSkyblock/modules/Chunklock/config.yml
```

Remember the two **core** settings Chunklock depends on:
`world.generator: normal` and `border.enabled: false` (see
[Installation](installation.md)).

---

## `start`

```yaml
start:
  radius-chunks: 0
```

How many rings around the center start open. `0` = only the center chunk (4 sides
locked); `1` = a 3×3 area open.

---

## `chunk-lock`

```yaml
chunk-lock:
  message-cooldown-ms: 1500
```

Minimum gap between "this chunk is not open" notices, so pushing against a locked
edge doesn't spam chat. `0` = show every time.

---

## `island-border`

```yaml
island-border:
  enabled: true
  warning-distance: 0
  warning-time: 0
```

| Key | Meaning |
|-----|---------|
| `enabled` | Show the island-upgrade WorldBorder (grows with the size upgrade). `false` = only the chunk lock runs |
| `warning-distance` | Red vignette distance. `0` disables it so the border stays purely informative |
| `warning-time` | Warning time. `0` disables |

---

## `barrier`

```yaml
barrier:
  material: BARRIER
  min-y: -128
  max-y: 500
```

The visible wall built on the edge of a locked chunk. Only **passable** blocks
become barrier (terrain is preserved); on unlock they revert to air.

| Key | Meaning |
|-----|---------|
| `material` | Block used for the wall |
| `min-y` / `max-y` | Vertical span of the wall |

<Callout type="info" title="The wall is only visual">

The real confinement is movement blocking (every height). The barrier is just
the wall players see and bump into.

</Callout>

---

## `hologram`

```yaml
hologram:
  y-offset: 2.0
  locked-title: "&c&l\U0001F512 Locked"
  free-line: "&7Drop: &f-"
  requirement-line: "&e{item} &7{have}&8/&e{amount}"
  footer: "&8(drop item)"
```

The floating marker over a locked frontier chunk.

| Key | Meaning |
|-----|---------|
| `y-offset` | Height above ground (blocks) |
| `locked-title` | Top line |
| `free-line` | Shown when the chunk is free |
| `requirement-line` | Per-item line: `{item}` `{have}` `{amount}` |
| `footer` | Bottom line |

Leave a line as `""` to hide it. `&` color codes are supported.

---

## `unlock`

```yaml
unlock:
  sound:
    enabled: true
    type: ENTITY_PLAYER_LEVELUP
    volume: 1.0
    pitch: 1.4

  items:
    - max-ring: 2
      require: [ DIRT:5 ]
    - max-ring: 4
      require: [ COBBLESTONE:16, OAK_LOG:8 ]
    - max-ring: 8
      require: [ IRON_INGOT:8, COAL:16 ]
    - max-ring: 16
      require: [ DIAMOND:4, GOLD_INGOT:8 ]
    - max-ring: 999999
      require: [ NETHERITE_INGOT:1, DIAMOND_BLOCK:2, PAPER:1001:1 ]
```

### `sound`

Played when a chunk unlocks. `type` is any Bukkit `Sound` enum name.

### `items`: cost by ring

A list of **bands**. For a chunk at ring R, the **first** band whose `max-ring >= R`
is used. `require` is a list of items the player must drop, in one of two forms:

| Form | Meaning |
|------|---------|
| `MATERIAL:amount` | Any item of that material (e.g. `DIAMOND:4`) |
| `MATERIAL:custom-model-data:amount` | Only items carrying **exactly** that `CustomModelData` (e.g. `PAPER:1001:1`) |

The 3-part form is for **resource pack / ItemsAdder / Oraxen** custom items: a
plain paper won't count toward a `PAPER:1001:1` requirement, only the modelled
one will.

Order bands from smallest `max-ring` to largest, and end with a catch-all
(`max-ring: 999999`) for the outer frontier.

| Band | Covers rings | Cost |
|------|--------------|------|
| 1 | 1–2 | 5× Dirt |
| 2 | 3–4 | 16× Cobblestone, 8× Oak Log |
| 3 | 5–8 | 8× Iron Ingot, 16× Coal |
| 4 | 9–16 | 4× Diamond, 8× Gold Ingot |
| 5 | 17+ | 1× Netherite Ingot, 2× Diamond Block, 1× custom Paper (model 1001) |

---

## `messages`

```yaml
messages:
  prefix: "&8[&aChunklock&8] &7"
  no-island: "&cYou need to create an island first: &f/ada oluştur"
  chunk-locked: "&cThis chunk is not open yet. &7Unlock it first."
  unlocked: "&aChunk unlocked! &7({x}, {z}) — ring {ring}"
  # ... and more
```

Every player-facing Chunklock string. Placeholders: `{x}` `{z}` `{ring}` `{count}`
`{item}` `{have}` `{amount}`. `&` color codes supported.
