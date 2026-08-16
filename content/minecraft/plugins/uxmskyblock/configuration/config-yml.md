---
title: config.yml
order: 730
description: 'config.yml is the master settings file: world, commands, storage, proxy,
  warps, generators, and more. This page walks through every section.'
---

<Callout type="tip" title="Updates merge automatically">

New keys from a plugin update are added to your existing file on load, so
upgrading never wipes your edited values.

</Callout>

---

## `island`

```yaml
island:
  enabled: true
  clear-inventory-on-leave: false
```

| Key | Meaning |
|-----|---------|
| `enabled` | Master switch for the island system. Set **false** on a server that should run only a module (e.g. a pure [Chunklock](../../chunklock/overview.md) server): the `/island` command, island listeners and the island world stay off, but core managers (economy, block values, storage, proxy, module loader) keep running. |
| `clear-inventory-on-leave` | Clear a member's inventory when they leave or are kicked, so they can't carry off island loot. |

---

## `world`

```yaml
world:
  name: islands
  generator: void        # void | normal
  island-height: 100
  island-distance: 250
  island-size: 100
```

| Key | Meaning |
|-----|---------|
| `name` | Island world name |
| `generator` | `void` = classic empty uxmSkyblock; `normal` = real terrain (required by Chunklock) |
| `island-height` | Y level islands are built at |
| `island-distance` | Spacing between island grid cells |
| `island-size` | Default protected size when the size upgrade isn't present |

---

## `nether` & `end`

```yaml
nether:
  enabled: true
  world-name: islands_nether
  island-height: 64
  schematic: nether.schem
  home-offset:
    x: 6.5
    y: 3.0
    z: 6.5
    yaw: 0.0
end:
  enabled: true
  world-name: islands_the_end
  island-height: 64
  schematic: end.schem
  home-offset:
    x: 7.5
    y: 4.0
    z: 10.5
    yaw: 180.0
```

Every island gets its **own** Nether and End plot, placed at the same grid cell as
the overworld island but in dedicated void worlds. Players reach them with **real
portals**: light the Nether portal in your Nether plot to dive in, then step
through the ready-made End portal that waits there to reach your End plot.

| Key | Meaning |
|-----|---------|
| `enabled` | Turn the dimension on/off |
| `world-name` | The dedicated void world for that dimension |
| `island-height` | Y level the plot is built at |
| `schematic` | `.schem` file pasted for the starter plot (in `plugins/uxmSkyblock/schematics/`). Leave blank to use the built-in block-by-block builder instead |
| `home-offset` | Where the player lands relative to the plot, with facing `yaw` |

<Callout type="note" title="Where the plots live">

The plots are built the first time a player travels there and live on the
**same backend server** as the island, so this works on a proxy network with no
extra setup. The bundled `nether.schem` / `end.schem` are extracted to the
schematics folder automatically and can be replaced with your own builds.

</Callout>

<Callout type="tip" title="No FastAsyncWorldEdit?">

If FAWE is missing, the plots fall back to a built-in builder that lays the
same layout block-by-block, so portals still work.

</Callout>

---

## `border`

```yaml
border:
  enabled: true
  color: BLUE            # BLUE | GREEN | RED
```

A per-player world border drawn when entering an island.

<Callout type="warning" title="Disable for Chunklock">

When the Chunklock module manages its own border, set `border.enabled: false`
to avoid two WorldBorders fighting.

</Callout>

---

## `spawn`

```yaml
spawn:
  world: ""
  x: 0.5
  y: 100
  z: 0.5
  yaw: 0
  pitch: 0
```

Where players go after their island is deleted (and on spawn return). Leave
`world` empty to use the main world spawn. Quick way to set it: stand somewhere and
run `/is anaspawn`. On a proxy network this can be overridden by `proxy.spawn-server`.

---

## `creation`

```yaml
creation:
  max-concurrent: 3
  threads: 3
```

How many islands may be built at once, and how many worker threads do it.

---

## `command`

The command name, aliases, and every sub-command word are configurable here. See
[Commands Overview](../commands/overview.md) for the full list.

```yaml
command:
  name: island
  aliases: [is, sb, ada, sky]
  subcommands:
    git: ["home", "go", "tp", "git", "ışınlan"]
    olustur: ["create", "oluştur", "olustur", "kur"]
    # ... one entry per sub-command
  invite-expire-seconds: 120
```

The left key is the **internal** name; the list is what players may type. There is
no command in `plugin.yml`: they're registered dynamically.

---

## `warp`

```yaml
warp:
  sign-tag: "[warp]"
  sign-display: "&9[Warp]"
  default-limit: 1
  permission-prefix: "skyblock.warps"
```

See [Warps](../commands/warps.md) for sign tags and per-rank limits.

---

## `economy`

```yaml
economy:
  enabled: true
```

When enabled (and Vault is present), upgrades charge `required-money`. With Vault
missing or this set to false, **only the level requirement applies**.

---

## `obsidian`

```yaml
obsidian:
  bucket-to-lava: true
```

Right-click obsidian with an empty bucket to recover the lava. See
[Generators](../progression/generators.md).

---

## `top`

```yaml
top:
  chat-lines: 10
  hologram:
    lines: 10
    height: 2.8
    refresh-ticks: 40
    title: "&6&lTOP ISLANDS"
    entry: "&7{rank}. &f{owner} &8- &a{level} &7({points})"
    empty: "&7No islands yet."
```

Leaderboard and the following hologram, see [Leaderboard](../commands/leaderboard.md).

---

## `schematics`

```yaml
schematics:
  default: classic
  list:
    classic:
      display-name: "&aClassic"
      file: classic.schem
      icon: GRASS_BLOCK
      home-offset: { x: 10.5, y: 11.0, z: 10.5 }
    desert:
      display-name: "&eDesert"
      file: desert.schem
      icon: SAND
      home-offset: { x: 0.5, y: 5.0, z: 0.5 }
```

The island types offered by `/is create [type]`. Each points to a `.schem` file
(used on `void` worlds via FastAsyncWorldEdit) and a home offset.

| Key | Meaning |
|-----|---------|
| `display-name` | Name shown in the type-selection menu |
| `file` | The `.schem` file in `plugins/uxmSkyblock/schematics/` |
| `icon` | Material shown for this type in the selection menu |
| `home-offset` | Where the player lands on the pasted island |

When a player runs `/is create` with **no type**, a selection menu lists every
entry here as its own icon, see [Island Type menu](../menus/island-type.md). Add a
new entry and it appears in the menu automatically.

---

## `storage`

```yaml
storage:
  type: sqlite           # sqlite | mysql
  mysql:
    host: localhost
    port: 3306
    database: skyblock
    username: root
    password: ""
    ssl: false
```

See [Database](../database/sqlite.md).

---

## `proxy`

Cross-server sync. Disabled by default. See the dedicated
[Cross-Server Setup](../proxy/cross-server.md) page for the full breakdown.

```yaml
proxy:
  enabled: false
  server-name: "skyblock-1"
  spawn-server: "spawn-1"
  create-servers: ["skyblock-1", "skyblock-2"]
  pending-teleport-seconds: 30
  debug: false
  redis:
    host: "localhost"
    port: 6379
    channel: "skyblock:proxy"
```
