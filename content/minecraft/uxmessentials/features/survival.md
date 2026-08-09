---
title: Survival Mechanics
order: 1460
description: The survival module is a grab-bag of opt-in gameplay conveniences — tree-feller,
  veinminer, auto-pickup, one-player sleep and more — that most servers reach for
  a separate plugin (or three) to get. Each mechanic is an independent switch, so
  you turn on only the ones your server wants and leave the rest dark. It's driven
  by a single HOCON file, modules/survival/config.conf.
---

The module ships **disabled**, because every mechanic in it changes how vanilla blocks
break, drop or replant, and that is a decision about your server's gameplay rather than
something an essentials plugin should make for you. Set `enabled = true` at the top of
the file and run `/uxmess reload survival` to turn it on; the individual mechanics then
follow their own switches under their own blocks.

---

## How a mechanic is controlled

Most mechanics answer to up to three layers, and it helps to keep them straight:

1. **The config switch** (`<mechanic> { enabled = ... }`) — the server operator's
   master on/off for that mechanic. Off here and the mechanic does nothing for anyone.
2. **A personal `/command` toggle** — seven of the mechanics let each player opt
   themselves out with a command (for example `/treefeller`). These default **on**, so
   a player who never touches the command has the mechanic active; running it once
   turns it off, again on. The choice is remembered per player.
3. **A permission node** — `uxmessentials.survival.<mechanic>` decides whether the
   mechanic acts for a player at all, and `.toggle` decides whether they may flip their
   personal switch. Everything is self-service (`default: true`), so the mechanics work
   out of the box and a player (or a group) opts out.

<Callout type="tip" title="One panel for every toggle">

`/survival` opens a settings panel that gathers all seven personal toggles in one
place — the same switches as the individual commands, laid out as a GUI. It's gated
by `uxmessentials.survival.gui`.

</Callout>

Not every mechanic has all three layers. Fast leaf decay, the anvil unlocker,
one-player sleep and head drops are world-side effects with no per-player toggle; farm
assist has a permission but no toggle command. Each section below spells out what it
carries.

---

## Tree-Feller

Break one log of a tree and the whole connected trunk of the same wood comes down with
it, up to `max-blocks`. Optionally replants a sapling at the base.

```hocon
tree-feller {
  enabled = true
  require-axe = true
  durability-drain = true
  durability-multiplier = 1
  max-blocks = 64
  hunger-cost = 0.0
  sneak-required = false
  replant-saplings = true
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `require-axe` | Require an axe in hand to fell. With this off, breaking a log by any means fells the tree. |
| `durability-drain` | Drain the axe's durability as the tree comes down. |
| `durability-multiplier` | Durability points spent per **extra** log felled (the log you broke by hand isn't counted). `1` mirrors mining each log normally; raise it to wear the axe faster. |
| `max-blocks` | The most logs a single fell brings down, counting the one you broke. A safety cap so an enormous custom tree can't lag the server. |
| `hunger-cost` | Food exhaustion added per extra log felled. `0.0` disables the hunger cost. |
| `sneak-required` | Doubles as the "shift to fell" switch. On: a normal break chops one log and a shift-break fells the tree. Off (default): any log break fells. |
| `replant-saplings` | Replant a matching sapling at the base when the block below is soil. Species without a plantable sapling (the nether stems) replant nothing. |

**Command:** `/treefeller` — flip your personal tree-feller on or off.

**Permissions:** `uxmessentials.survival.treefeller` (the mechanic acts for you),
`uxmessentials.survival.treefeller.toggle` (you may switch it). Both `default: true`.

---

## Veinminer

Break one of the listed blocks and the whole connected vein of that same material
breaks with it, up to `max-blocks`.

```hocon
veinminer {
  enabled = true
  blocks = [
    "COAL_ORE", "DEEPSLATE_COAL_ORE",
    "IRON_ORE", "DEEPSLATE_IRON_ORE",
    "COPPER_ORE", "DEEPSLATE_COPPER_ORE",
    "GOLD_ORE", "DEEPSLATE_GOLD_ORE",
    "REDSTONE_ORE", "DEEPSLATE_REDSTONE_ORE",
    "LAPIS_ORE", "DEEPSLATE_LAPIS_ORE",
    "DIAMOND_ORE", "DEEPSLATE_DIAMOND_ORE",
    "EMERALD_ORE", "DEEPSLATE_EMERALD_ORE",
    "NETHER_GOLD_ORE", "NETHER_QUARTZ_ORE",
    "ANCIENT_DEBRIS"
  ]
  max-blocks = 64
  tool-durability = true
  hunger-cost = 0.0
  sneak-required = true
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `blocks` | The blocks a vein-break triggers on, by Bukkit material name. Breaking any of these mines the connected run of the same block. Unknown names are skipped with a warning at startup. |
| `max-blocks` | The most blocks a single vein-break mines, counting the one you broke. A safety cap for very large veins. |
| `tool-durability` | Drain the mining tool's durability as the vein breaks. |
| `hunger-cost` | Food exhaustion added per extra block mined. `0.0` disables the hunger cost. |
| `sneak-required` | The "shift to vein" switch. On (default): a plain break takes one ore and a shift-break takes the vein. Off: any break of a listed block veins it. |

**Command:** `/veinminer` — flip your personal veinminer on or off.

**Permissions:** `uxmessentials.survival.veinminer`,
`uxmessentials.survival.veinminer.toggle`. Both `default: true`.

---

## Fast Leaf Decay

When a log is broken, the leaves it was holding up decay right away instead of
lingering for vanilla's slow random-tick sweep. Only leaves that a standing trunk no
longer supports come down, and **player-placed (persistent) leaves are never touched**.

```hocon
leaf-decay {
  enabled = true
  radius = 4
  delay-ticks = 4
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `radius` | How far from the broken log to look for leaves, as the half-width of a cube. Larger clears a bigger canopy but costs more to sweep — keep it modest. |
| `delay-ticks` | Ticks to wait after the log breaks before decaying the leaves (20 ticks = 1 second). A small delay lets the cascade read as a gentle ripple rather than the whole canopy vanishing at once. |

<Callout type="info" title="No player toggle">

Fast leaf decay is a world-side effect — there is no `/command` toggle and no
permission node. It's on or off for the whole server via `enabled`.

</Callout>

---

## Farmland Protection

A personal toggle that stops **you** trampling farmland into dirt, whether by stepping
on it or landing on it from a fall. Other players and mobs are unaffected.

```hocon
farmprotect {
  enabled = true
}
```

**Command:** `/farmprotect` — switch your personal farmland protection on or off.

**Permissions:** `uxmessentials.survival.farmprotect`,
`uxmessentials.survival.farmprotect.toggle`. Both `default: true`.

---

## Farm Assist

Right-click a fully grown crop to harvest its drops and replant it in one motion.
Replanting spends one matching seed from your inventory (wheat seeds for wheat, a carrot
for carrots, and so on); with no matching seed the crop is harvested but not replanted.

```hocon
farmassist {
  enabled = true
  crops = [
    "WHEAT", "CARROTS", "POTATOES", "BEETROOTS", "NETHER_WART"
  ]
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `crops` | The crops farm assist acts on, by Bukkit material name. Only crops that grow through a block age and replant from a single seed item are supported; unknown or seedless crops are skipped with a warning at startup. |

<Callout type="info" title="Permission-gated, no toggle command">

Farm assist has **no** personal toggle command. It acts for any player holding
`uxmessentials.survival.farmassist` (`default: true`) — negate that node to deny it
to a player or group.

</Callout>

---

## Anvil Unlocker

Lift vanilla's anvil caps so a costly combine is not rejected with *"Too Expensive!"*.
Purely a convenience tweak on the anvil screen.

```hocon
anvilunlocker {
  enabled = true
  remove-level-limit = true
  remove-cost-limit = false
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `remove-level-limit` | Lift the "Too Expensive!" ceiling (vanilla stops a survival combine once it costs 40 levels) so any combine, however many prior-work levels it has stacked up, produces a result. |
| `remove-cost-limit` | Also zero the level price of the combine, so the unlocked repair is free rather than charging the full XP cost. **Off** by default, so an unlocked combine still costs its levels. |

<Callout type="info" title="Applies to every anvil">

There is no per-player toggle or permission — while enabled, the unlock applies to
every anvil on the server.

</Callout>

---

## One-Player Sleep

Skip the night as soon as enough of a world's players are sleeping, evaluated per world.
AFK players (those the presence module marks sleep-ignored) and spectators neither count
toward nor block the skip.

```hocon
oneplayersleep {
  enabled = true
  required-percent = 50
  required-count = 1
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `required-percent` | The percentage of eligible players that must be sleeping to skip the night, rounded up to a whole player. Only used when `required-count` is `0`. |
| `required-count` | A fixed number of sleeping players that skips the night. Above `0` this takes precedence over `required-percent` entirely — the default of `1` is the classic one-player sleep. Set to `0` to fall back to the percentage instead. |

---

## Head Drops

A slain entity may drop its head. A player killed by another player drops their own
head; mobs drop their vanilla head at a chance.

```hocon
headdrop {
  enabled = true
  player-head-on-pvp = true
  mob-chance = 0.0
  mobs {
    # WITHER_SKELETON = 100.0
    # ZOMBIE = 25.0
  }
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `player-head-on-pvp` | A player killed by another player drops their player head at full odds. PvE and environmental deaths never drop a player head. |
| `mob-chance` | The default chance (a percentage, 0–100) a slain mob drops its head. `0.0` means mobs drop no heads unless a per-mob override raises their chance. |
| `mobs` | Per-mob chance overrides, keyed by Bukkit entity name. A listed mob uses its own chance instead of `mob-chance`; unknown names are skipped with a warning at startup. |

<Callout type="note" title="Only mobs with a real head item">

Chances only matter for the mobs that actually have a Minecraft head: zombie,
skeleton, wither skeleton, creeper, piglin, and the ender dragon.

</Callout>

---

## Auto-Pickup

A broken block's drops go straight into your inventory instead of scattering on the
ground; only when the inventory is full do the extras spill out.

```hocon
autopickup {
  enabled = true
  transfer-xp = false
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Default `true`. |
| `transfer-xp` | Grant the block's dropped experience straight to the player instead of spawning XP orbs. **Off** by default, so mining still drops the usual orbs. |

**Command:** `/autopickup` — flip your personal auto-pickup on or off.

**Permissions:** `uxmessentials.survival.autopickup`,
`uxmessentials.survival.autopickup.toggle`. Both `default: true`.

<Callout type="info" title="Blocks that hold something are never touched">

A chest, barrel, furnace, hopper, dropper, dispenser, brewing stand, lectern, campfire or
any other block with contents is left entirely to vanilla: auto-pickup, auto-smelt and
auto-sell skip it, and a tree-fell or vein that reaches one breaks it normally.

This is not a limitation, it is the correct behaviour. What is inside such a block is
dropped by the server as the block is removed, in the same batch as the block's own item,
so a mechanic that takes that batch over would delete the contents. Breaking a full chest
drops its items exactly as it does on a vanilla server. The only thing you give up is the
chest itself landing in your inventory instead of at your feet.

</Callout>

---

## Auto-Smelt

A broken block's smeltable drop is turned into its furnace result as you mine, so an
iron ore yields an ingot rather than raw iron. It runs **before** auto-pickup, so what
lands in your inventory is already smelted.

```hocon
autosmelt {
  enabled = true
  smelt {
    RAW_IRON = IRON_INGOT
    RAW_GOLD = GOLD_INGOT
    RAW_COPPER = COPPER_INGOT
    ANCIENT_DEBRIS = NETHERITE_SCRAP
    # COBBLESTONE = STONE
    # COBBLED_DEEPSLATE = DEEPSLATE
    # SAND = GLASS
    # RED_SAND = GLASS
    # CLAY_BALL = BRICK
    # WET_SPONGE = SPONGE
    # KELP = DRIED_KELP
    # CHORUS_FRUIT = POPPED_CHORUS_FRUIT
  }
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Ships `true`. Setting it to `false` removes the mechanic **and** the `/autosmelt` command from the server. |
| `smelt` | The drop → smelted-result map, keyed by the **drop** material (raw iron, not the ore block) using Bukkit material names. Delete an entry to stop smelting that drop; add your own to smelt more. The shipped set is ores only. |

**Command:** `/autosmelt` — flip your personal auto-smelt on or off.

**Permissions:** `uxmessentials.survival.autosmelt`,
`uxmessentials.survival.autosmelt.toggle`. Both `default: true`.

<Callout type="warning" title="Think twice before smelting cobblestone">

The commented-out pairs above are a bigger change than they look. `COBBLESTONE =
STONE` takes cobblestone off your server entirely: nobody can build with it, and a
cobblestone generator stops producing anything usable. The same goes for sand to
glass and clay to brick. Uncomment them only if that is genuinely the server you
want.

</Callout>

---

## Auto-Sell

A broken block's priced drops are sold into the server economy as you mine and the coins
credited to your wallet. It runs after auto-smelt, so it prices the smelted result.

```hocon
autosell {
  enabled = true
  notify {
    mode = "actionbar"
    interval-seconds = 3
  }
  prices {
    COAL = 2
    IRON_INGOT = 8
    COPPER_INGOT = 4
    GOLD_INGOT = 12
    REDSTONE = 1
    LAPIS_LAZULI = 2
    DIAMOND = 80
    EMERALD = 60
    QUARTZ = 3
    NETHERITE_SCRAP = 120
  }
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch. Ships `true`, but nothing is sold until a player opts in: unlike pickup and smelt, the **personal** `/autosell` toggle defaults **off**. |
| `notify.mode` | Where the sale receipt goes: `actionbar` (the default), `chat`, or `off` for no notice at all. |
| `notify.interval-seconds` | Sales are pooled for this long and reported as one line, so a vein does not print a receipt per block. `0` reports every sale on its own; raise it for `chat` mode. |
| `prices` | Per-item sell prices, keyed by the (post-smelt) drop material using Bukkit material names. The value is the price for **one** item; a stack sells for that times its size. A drop with no listed price is never sold: it falls through to auto-pickup or the ground. |

**The sale receipt.** A sold drop never reaches your inventory, so with no word from the
plugin it looks exactly like a lost item. Every sale is reported as what it sold and what it
paid, pooled over the window: `12x Iron Ingot, 3x Coal for $102`. Item names come from the
client, so each player reads them in their own language, and the money figure is formatted
by the economy exactly as `/balance` prints it.

**Command:** `/autosell` — flip your personal auto-sell on or off.

**Permissions:** `uxmessentials.survival.autosell`,
`uxmessentials.survival.autosell.toggle`. Both `default: true`.

<Callout type="warning" title="Needs the economy module">

Auto-sell does nothing unless the [economy](economy.md) module is present: it needs
a wallet to credit. Price the materials you want sellable, and tell players to run
`/autosell` to switch it on for themselves.

</Callout>

---

## Auto-Tool

As you start breaking a block, your held hotbar slot swaps to the strongest tool of the
family that block needs — a pickaxe for stone, an axe for a log. It only ever swaps
between hotbar slots you **already** carry.

```hocon
autotool {
  enabled = true
}
```

| Key | What it does |
|-----|--------------|
| `enabled` | Master switch for the mechanic. Ships `true`. Setting it to `false` removes the mechanic **and** the `/autotool` command from the server. |

**Command:** `/autotool` — flip your personal auto-tool on or off.

**Permissions:** `uxmessentials.survival.autotool`,
`uxmessentials.survival.autotool.toggle`. Both `default: true`.

---

## Commands and Permissions at a Glance

| Mechanic | Config block | Command | Use node | Toggle node |
|----------|--------------|---------|----------|-------------|
| Tree-feller | `tree-feller` | `/treefeller` | `uxmessentials.survival.treefeller` | `.treefeller.toggle` |
| Veinminer | `veinminer` | `/veinminer` | `uxmessentials.survival.veinminer` | `.veinminer.toggle` |
| Fast leaf decay | `leaf-decay` | — | — | — |
| Farmland protection | `farmprotect` | `/farmprotect` | `uxmessentials.survival.farmprotect` | `.farmprotect.toggle` |
| Farm assist | `farmassist` | — | `uxmessentials.survival.farmassist` | — |
| Anvil unlocker | `anvilunlocker` | — | — | — |
| One-player sleep | `oneplayersleep` | — | — | — |
| Head drops | `headdrop` | — | — | — |
| Auto-pickup | `autopickup` | `/autopickup` | `uxmessentials.survival.autopickup` | `.autopickup.toggle` |
| Auto-smelt | `autosmelt` | `/autosmelt` | `uxmessentials.survival.autosmelt` | `.autosmelt.toggle` |
| Auto-sell | `autosell` | `/autosell` | `uxmessentials.survival.autosell` | `.autosell.toggle` |
| Auto-tool | `autotool` | `/autotool` | `uxmessentials.survival.autotool` | `.autotool.toggle` |

<Callout type="warning" title="Turning a mechanic off also removes its command">

Each `enabled` key above owns both the mechanic and its command. With
`autosmelt.enabled = false`, `/autosmelt` is never registered at all, so a player who
types it gets *unknown command* rather than a "this is off" reply. If a survival
command seems to be missing, check that block's `enabled` first.

</Callout>

`/survival` opens the settings panel that gathers the seven personal toggles, gated by
`uxmessentials.survival.gui`. All the nodes above default `true` (self-service). The
administrative node `uxmessentials.module.survival` (`op`) governs who may hot-reload or
inspect the module via `/uxmess reload survival`.

---

## Next Steps

- [🧩 Per-Module Config](../config/per-module.md) — where `modules/survival/config.conf` lives and how it's loaded
- [💰 Economy](economy.md) — the wallet auto-sell credits into
- [🔑 Permission Reference](../permissions/reference.md) — the full `uxmessentials.survival.*` node list
