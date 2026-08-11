---
title: Ranks & Prestige
order: 1250
description: 'The ranks module is the server''s progression ladder: a chain of ranks a
  player climbs with /rankup, an optional /prestige reset at the very top, and an
  optional autorank scan that promotes eligible players automatically. The plugin tracks
  each player''s current rank itself (a DB-backed pointer that survives a world
  rollback) so rankup does not depend on a permission plugin. A rank''s rankup actions
  can set a LuckPerms group, run any command, or do nothing at all; that is entirely up
  to you.'
---

The module ships **enabled but inert**: nothing happens until you author a ladder in
`ranks.conf`. It is driven by three files under `modules/ranks/`: `config.conf`
(the module toggle plus the prestige, autorank and GUI switches), `ranks.conf` (the
ladder itself), and `gui/ranks-panel.conf` (the `/ranks` panel layout).

---

## The Rank Ladder

The ladder lives in `ranks.conf`. Each top-level section is one rank; the **section
name is the rank's id**: the value stored against the player and referenced by other
ranks. Ranks are ordered by their `order` number, lowest first, so a player on
`first` ranks up to `citizen`, then to `vip`.

```hocon
first {
  order = 10
  display-name = "<gray>First</gray>"
  cost = 0
  requirements = []
  actions = []
}

citizen {
  order = 20
  display-name = "<green>Citizen</green>"
  cost = 1000
  requirements = [
    "money 1000"
  ]
  actions = [
    "message <green>Welcome to Citizen! Keep it up."
    "console say {player} climbed to Citizen!"
    "sound minecraft:entity.player.levelup"
  ]
}

vip {
  order = 30
  display-name = "<gold>VIP</gold>"
  cost = 5000
  requirements = [
    "money 5000"
    "playtime 3600"
  ]
  actions = [
    "message <gold>You are now VIP — thanks for playing!"
    "console say {player} is now VIP!"
    "sound minecraft:ui.toast.challenge_complete"
  ]
}
```

Reading the `vip` rank above: to reach it a player must already be on `citizen`
(the previous rung), be able to afford `5000` and have at least an hour of tracked
playtime; once they rank up, `5000` is charged, they see a message and a sound, and
the server announces it in chat.

| Key | What it does |
|-----|--------------|
| `order` | The ladder position, ascending: lower ranks up to higher. Leave gaps (`10`, `20`, `30`) so you can insert a rank later without renumbering the rest. |
| `display-name` | How the rank is shown in messages and the GUI. MiniMessage / legacy colour codes are allowed. |
| `cost` | How much is charged through the economy when the player ranks up (`0` for a free rank). |
| `requirements` | What the player must satisfy to rank up (each entry `"<type> <value>"`, checked on `/rankup`). |
| `actions` | What runs when the player ranks up, in order (each entry `"<type> <value>"`). |

<Callout type="info" title="The ladder is unambiguous and gap-tolerant">

Rank ids are unique: a duplicate id is rejected on load. The ladder may be
empty (an operator who cleared `ranks.conf`), in which case `/rankup` and the
placeholders simply degrade rather than erroring.

</Callout>

### Requirements

Each `requirements` entry is `"<type> <value>"`, checked when the player runs
`/rankup`. An **unmet** requirement blocks the rankup; an **unknown** type (a typo,
or a value-less entry) is simply not enforced rather than failing the whole rank.
Any requirement that can't be verified: an offline player for an inventory or
placeholder check, a malformed number, an unknown material, or a money check with no
economy installed: **fails closed**, so a rankup never slips through on a condition
that couldn't be checked.

| Type | Form | Passes when |
|------|------|-------------|
| `money` | `money <amount>` | The player can **afford** the amount in the default currency (a balance check, not a charge: the charge is the rank's `cost`). |
| `playtime` | `playtime <seconds>` | Tracked playtime is at least this many whole seconds. |
| `permission` | `permission <node>` | The player holds the permission node. |
| `previous-rank` | `previous-rank <id>` | The player is currently on that rank (the previous rung). Aliases: `prev-rank`, `prevrank`. |
| `item` | `item <material> [n]` | The player's inventory holds `n` of the material (default `1`). |
| `placeholder` | `placeholder <a> <op> <b>` | A `%placeholder%` comparison: `op` is one of `== != > < >= <=`. Needs PlaceholderAPI. |

### Actions

Each `actions` entry is `"<type> <value>"` and runs, in order, once the rankup is
durable. Actions use the **shared click-action grammar**: the same one NPCs and
holograms run their interaction chains through, so anything you can do on a click,
you can do on a rankup. Use `{player}` for the player's name. A line whose type word
is unknown, or that carries no value, is logged and skipped so one typo never aborts
the rest of the rank's actions.

| Type | Effect |
|------|--------|
| `console <command>` | Run the value as a command from the server console. |
| `player <command>` | Run the value as a command performed by the player. |
| `player_op <command>` | Run the command as the player with a temporary op grant for that one dispatch. |
| `message <text>` | Send a MiniMessage chat line to the player. |
| `actionbar <text>` | Show the value on the player's action bar. |
| `title <title\|subtitle\|fadeIn\|stay\|fadeOut>` | Show a title (subtitle and timings optional). |
| `sound <key[:volume[:pitch]]>` | Play a sound to the player. |
| `connect <server>` | Send the player to another server through the proxy. |

The grammar's flow-control and gate types (`delay`, `random`, `chance`,
`permission`, `condition`, `give`) work here too, see
[Actions & Requirements](../menus/actions-requirements.md) for the full list.

<Callout type="tip" title="Set a permission-plugin group on rankup">

Because the plugin owns the rank pointer itself, wiring a rank into your
permission plugin is just another action:

```hocon
actions = [
  "console lp user {player} parent set citizen"
]
```

This is left out of the shipped example ranks so a fresh server without LuckPerms
stays quiet: add it once your groups are set up.

</Callout>

<Callout type="note" title="A rank's paywall is its `cost`, not a `cost` action">

The rank action runner is wired **without** an economy of its own, so a `cost`
gate placed in an `actions` list is a no-op. Charge for a rank through its `cost`
field, which is debited before the pointer advances.

</Callout>

---

## The Rankup Flow

`/rankup` advances the invoking player one rung up the ladder. The steps run in a
deliberate order so a refused rankup leaves no trace:

1. **Requirements**: every parsed requirement of the next rank is checked first. A
   single failure refuses with nothing charged and the pointer unmoved.
2. **Charge**: only once the requirements pass is the rank's `cost` charged through
   the economy. A short balance refuses; no money moves and the pointer does not
   advance. With no economy provider wired, or a zero cost, the charge is skipped and
   the rankup is free.
3. **Advance**: the rank pointer is saved (the prestige level is carried over
   unchanged).
4. **Actions**: the rank's actions run last, only after the advance is durable.

The feedback the player sees maps to the outcome: ranked up (naming the new rank),
already at the highest rank, requirements not met, or can't afford.

`/rankup` is gated on `uxmessentials.ranks.rankup`, which ships `true` so every
player may try to progress.

---

## Prestige

Prestige lets a player **at the very top rank** reset back to the first rank in
exchange for a permanent prestige level (and whatever reward you configure). It ships
**on**, though it does nothing until a player actually reaches the top of your ladder.
Set `enabled = false` and the `/prestige` command is not registered at all.

```hocon
prestige {
  enabled = true
  max-level = 0          # highest reachable level; 0 = no cap (prestige forever)
  cost = 0               # charged through the economy on prestige (0 = free)
  requirements = []      # same grammar as a rank's requirements
  actions = []           # same grammar as a rank's actions
  reward-multiplier = 1.0
}
```

`/prestige` mirrors the rankup pipeline: the player must be at the top rank, below
the `max-level` cap, and past the configured `requirements`; then the `cost` is
charged, the pointer is reset to the first rank, the prestige level is incremented,
and the `actions` run. A refused prestige, not yet at the top, at the cap,
requirements unmet, or unaffordable: leaves the level and pointer untouched.

The `reward-multiplier` is applied **linearly per level**: `1.0` grants no bonus,
`1.5` gives a 1.5× multiplier at prestige 1, `2.0`× at prestige 2, and so on. It is
shown to the player on prestige and exposed through the `%uxmessentials_prestige%`
placeholder, so your reward systems can scale with prestige level.

`/prestige` is gated on `uxmessentials.ranks.prestige` (default `true`).

---

## Autorank

Autorank promotes online players automatically the moment they already meet their
next rank's requirements, without them running `/rankup`. It ships **off**.

```hocon
autorank {
  enabled = false
  interval-seconds = 300   # how often the scan runs (clamped to a minimum of 1)
  charge-cost = true       # whether an automatic promotion charges the rank's cost
}
```

When on, a scheduled scan checks every online player on the interval and advances
each eligible player **one rank per scan**: a player who qualifies for several ranks
at once climbs one per interval, so shorten the interval if you want faster climbing.
The scan reuses the very same `/rankup` pipeline, so requirements and (unless
`charge-cost = false`) the cost apply exactly as a manual rankup would.

<Callout type="info" title="Keep the interval comfortably large">

Every scan re-checks every online player's next-rank requirements. The default
`300` (five minutes) is a sensible balance; `interval-seconds` is clamped to a
minimum of `1` so a misconfigured `0` cannot spin the scheduler.

</Callout>

---

## The `/ranks` Panel

`/ranks` (with no argument) opens a self-service ladder panel: a three-row window
showing the player's current rank and prestige, the next rank on the ladder with its
cost and requirements, and a **rank-up button** that runs the same pipeline as
`/rankup`. It ships **on** (`gui.enabled = true`).

The rank-up button routes through the identical `/rankup` use case, so the panel and
the command always agree: a refused rankup leaves the standing unmoved, and the
panel redraws with the new standing after each click.

```hocon
gui {
  enabled = true
}
```

Set `gui.enabled = false` to drop the panel (and the no-argument `/ranks` open)
entirely, leaving only the admin `/ranks setrank`. The panel geometry and its icons
live in `gui/ranks-panel.conf` (the current-rank display defaults to a diamond, the
next rank to an experience bottle, the rank-up button to an emerald), and every label
resolves from the message catalog. Opening the panel is gated on the self-service
`uxmessentials.ranks.gui` (default `true`).

<Callout type="tip" title="Panel placeholders for a custom layout">

Inside `gui/ranks-panel.conf` the display items read the viewer's standing
through five menu placeholders, computed when the panel opens:
`{ranks_current}`, `{ranks_prestige}`, `{ranks_next}`, `{ranks_next_cost}` and
`{ranks_next_requirements}`. These are the panel's own tokens: distinct from the
server-wide PlaceholderAPI placeholders below.

</Callout>

---

## Placeholders

With PlaceholderAPI installed, three server-wide placeholders expose a player's
standing. All read the **DB-backed pointer**, so they resolve for an offline player
too. When the ranks module is disabled (or the ladder holds no ranks) each degrades
to a dash (`-`).

| Placeholder | Resolves to |
|-------------|-------------|
| `%uxmessentials_rank%` | The player's current rank display name |
| `%uxmessentials_rank_next%` | The next rank's display name: `max` when the player is already at the highest rank |
| `%uxmessentials_prestige%` | The player's prestige level (an integer) |

---

## Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/rankup` | Advance one rung up the ladder when you meet the next rank's requirements | `uxmessentials.ranks.rankup` |
| `/prestige` | Reset to the first rank for a prestige level once you reach the top rank (registered while prestige is enabled) | `uxmessentials.ranks.prestige` |
| `/ranks` | Open the ladder panel (registered only when the GUI is enabled) | `uxmessentials.ranks.gui` |
| `/ranks setrank <player> <rank>` | Set a player's rank pointer directly, bypassing requirements, cost and actions (prestige is kept) | `uxmessentials.ranks.admin` |
| `/setrank <player> <rank>` | The same thing as a top-level command | `uxmessentials.ranks.admin` |

<Callout type="warning" title="`setrank` is the escape hatch">

`setrank` writes the pointer straight to the named rank: no requirement check, no
cost, no actions. It is the deliberate tool for corrections and grants. A rank id
that isn't on the ladder is refused and nothing is written.

`/setrank` and `/ranks setrank` run exactly the same use case behind the same
`uxmessentials.ranks.admin` node. `/setrank` is a separate command id, so you can
rename or disable it on its own in `commands.conf` without touching `/ranks`.

</Callout>

---

## Permissions at a Glance

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.ranks.rankup` | `true` | `/rankup`: advance to the next rank |
| `uxmessentials.ranks.prestige` | `true` | `/prestige`: reset to the first rank for a prestige level |
| `uxmessentials.ranks.gui` | `true` | `/ranks`: open the self-service ladder panel |
| `uxmessentials.ranks.admin` | `op` | `/ranks setrank <player> <rank>`: set a player's rank directly |

The administrative node `uxmessentials.module.ranks` (`op`) governs who may hot-reload
or inspect the module via `/uxmess reload ranks`.

---

## Config at a Glance

| File | Owns |
|------|------|
| `config.conf` | The module toggle plus the `gui`, `prestige` and `autorank` switches |
| `ranks.conf` | The ladder: each rank's `order`, `display-name`, `cost`, `requirements` and `actions` |
| `gui/ranks-panel.conf` | The `/ranks` panel geometry and icons |

---

## Next Steps

- [🎛️ Actions & Requirements](../menus/actions-requirements.md): the full click-action grammar rank actions use
- [💰 Economy](economy.md): the wallet a rank's `cost` is charged against
- [🧩 Per-Module Config](../config/per-module.md): where `modules/ranks/` lives and how it's loaded
- [🔑 Permission Reference](../permissions/reference.md): the `uxmessentials.ranks.*` nodes
