---
title: Per-Module Config
order: 1160
description: Every feature in uxmEssentials is a module with its own folder under
  plugins/uxmEssentials/modules/. All of a feature's tunables live in that folder,
  and the module is switched on or off by the first key in its own config.conf. There
  is no central switchboard file (see the warning below).
---

## How to Edit

1. Open `plugins/uxmEssentials/modules/<module>/config.conf`.
2. Change the values you need — keep braces `{ }` and quotes balanced.
3. Save.
4. Apply it with a **targeted** reload:

```text
/uxmess reload <module>     # e.g. /uxmess reload homes
```

<Callout type="warning" title="HOCON, not YAML">

These files are HOCON: `#` comments, kebab-case keys, quoted strings, bare numbers
and booleans, `[ … ]` lists, `{ … }` blocks. Indentation is cosmetic; braces and
quotes are load-bearing.

</Callout>

<Callout type="danger" title="There is no `modules.conf`">

Modules are **not** enabled from a central file — that file does not exist. Each
module is toggled by the `enabled` key at the top of its own
`modules/<module>/config.conf`. Older notes that mention a `modules.conf`
switchboard are stale.

</Callout>

---

## The `enabled` first key

Open any module's `config.conf` and the very first key is `enabled`:

```hocon
# Homes — /home opens the slot-based GUI.
enabled = true
default-limit = 3
# … the rest of the homes settings …
```

- Set `enabled = false` to switch the whole module off. A disabled module registers
  **no commands, no listeners, runs no database migrations, and holds no runtime
  state** — it is genuinely inert, not just hidden.
- Set it back to `true` and `/uxmess reload <module>` to bring it up live.
- Disabling a module **never deletes stored data.** Its rows stay in the database and
  return untouched when you re-enable it.

Most modules start `enabled = true`. Ten start `enabled = false`: the HUD trio
(`scoreboard`, `tablist`, `nametags`), the gameplay-changing `survival` and
`villagers`, the ones that need an external service (`discordlink`, `vote`,
`regions`), the snapshot-writing `invrollback`, and the one-shot EssentialsX
importer (`migration`). Each one says why in the comment at the top of its own file.
`/uxmess status` prints the live state of every module, and
[Modules & Reloading](../modules/overview.md) has the full roster with the reasons.

---

## Big-block sub-files

A few modules lift a large section out of `config.conf` into a sibling file in the same
folder, purely to keep the main file readable. They are mounted as if they were part of
the module:

| Module | Sub-file | Holds |
|---|---|---|
| `teleport` | `rtp.conf` | `/rtp` safe-search tuning |
| `economy` | `currencies.conf` | Every currency definition |
| `communication` | `join-quit.conf`, `announcer.conf`, `info-pages.conf`, `advancements.conf` | The four communication feature blocks |

Editing a sub-file and reloading its module applies it the same as editing the main
`config.conf`.

The `gui/` sub-folder holds one file per in-game panel the module can open: the editors
behind `/uxmess gui`, but also every browser, picker and confirm screen the module's own
commands open. They are written in the same grammar as [custom menus](../menus/engine.md),
so a panel's height, backdrop, icons and wording are yours to change without touching
code. A few of them also hand a block of slots to the module for real item stacks; see
[Built-in Menus](../menus/built-in.md#windows-that-hold-real-items) before you rearrange
one of those.

---

## Example: `modules/homes/config.conf`

```hocon
enabled = true
default-limit = 3                   # homes per player without a limit permission node
limit-mode = "highest"              # combine limit tiers: "highest" or "stack"
unlimited-max = 1000                # slot ceiling for an unlimited (-1) quota
date-format = "dd/MM/yyyy HH:mm"    # how a home's creation time renders in the GUI
disabled-worlds = []                # worlds where homes may not be created/relocated
block-unsafe-sethome = true         # reject placing a home in an unsafe spot

economy {
  enabled = false                   # charge players for home actions
  create-cost = 0
  relocate-cost = 0
  teleport-cost = 0
  currency = "default"
}

claims {
  enabled = true                    # honour the active land-claim plugin
  require-claim = false
  block-foreign-claims = true
  check-teleport-access = true
}
```

The pattern repeats everywhere: `enabled` first, then flat tunables, then grouped blocks
for optional sub-systems (economy charges, claim integration, and so on).

---

## Example: `modules/teleport/config.conf`

```hocon
enabled = true
cooldown-start-phase = "teleport"   # request | accept | teleport
request-ttl-seconds = 60            # how long a /tpa request stays pending
default-warmup = 3                  # seconds, absent a warmup permission tier
default-cooldown = 5                # seconds, absent a cooldown permission tier
teleport-to-center = true           # snap the destination to the block centre

warmup {
  cancel-on-move = true
  cancel-on-rotate = false
  move-threshold = 0.0              # blocks of movement tolerated before cancelling
}

back {
  on-death = true                   # /back may return to a death location
  death-delay-seconds = 0
  ignored-causes = ["ender_pearl", "chorus_fruit"]
}
```

The `/rtp` safe-search lives beside it in `modules/teleport/rtp.conf`:

```hocon
min-radius = 100
max-radius = 5000
claim-aware = true                  # avoid landing on claimed land
queue-target-size = 10              # pre-validated locations kept ready per world
excluded-biomes = ["ocean", "deep_ocean", "river"]
avoid-blocks = ["lava", "magma_block", "fire", "cactus"]
```

---

## Example: `modules/economy/config.conf`

```hocon
enabled = true
amount-format = "full"              # full (grouped digits) | compact (1.2k style)

wallet {
  default-currency = "coins"        # id of the currency used when none is named
  starting-balance = "0"
  max-balance = "1000000000000"
  gui-enabled = true
}

bank {
  enabled = true                    # enable /bank shared accounts
}

pay {
  min-pay = "0.01"
}
```

Individual currencies (symbol, precision, starting balance, per-currency flags) are
defined in `modules/economy/currencies.conf`.

---

## Reload per module

`/uxmess reload <module>` re-reads **only** that module's subtree — its `config.conf`
plus any sub-files — restarts just that module off-thread, and re-publishes its commands.
It is faster and safer than a full reload: if you introduce a syntax error, only that one
module refuses to reload and the log points at the file.

<Callout type="tip" title="Deep tuning lives in the feature guides">

This page covers how the files are laid out. For what each key actually does per
feature, jump to the matching Feature Guide — for example
[Homes](../features/homes.md), [Teleport & RTP](../features/teleport-rtp.md),
[Economy](../features/economy.md), [Kits](../features/kits.md), or
[Moderation](../features/moderation.md).

</Callout>

---

## Next Steps

- [Modules & Reloading](../modules/overview.md) — the module roster and reload model.
- [config.conf (Globals)](global-config.md) — the settings that are *not* per-module.
- [Renaming Commands](commands-conf.md) — rename or disable any command per install.
- [Permission Reference](../permissions/reference.md) — the nodes each module reads.
