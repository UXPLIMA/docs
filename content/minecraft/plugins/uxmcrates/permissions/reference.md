---
title: Reference
order: 601
description: Every permission node of uxmCrates with its default, what it allows, and the nodes a server names itself.
icon: list-checks
---

`/crates` and its subcommands carry no node. A sender without an operator subcommand's node does not see it.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmcrates.language` | everyone | `/uxmcrates lang`, `lang set`, `lang reset` |
| `uxmcrates.broadcast.hide` | everyone | Hides other players' win broadcasts from the holder. `broadcast.opt-out-permission` renames it |
| `uxmcrates.cooldown.bypass` | op | Opens a crate with no wait, timed or one-time |

<Callout type="warning" title="Every player holds broadcast.hide">

`uxmcrates.broadcast.hide` defaults to `true`, so out of the box no player reads win broadcasts. Negate it for the
players who should: `lp group default permission set uxmcrates.broadcast.hide false`.

</Callout>

## Operators

| Node | Allows |
|---|---|
| `uxmcrates.admin.*` | Declared as the family, `op` by default. It lists no children, so grant the nodes below one by one, or use a permission plugin's wildcard |
| `uxmcrates.admin.version` | `/uxmcrates`, `/uxmcrates version` |
| `uxmcrates.admin.doctor` | `/uxmcrates doctor` |
| `uxmcrates.admin.reload` | `/uxmcrates reload` |
| `uxmcrates.admin.editor` | `/uxmcrates editor` |
| `uxmcrates.admin.place` | `/uxmcrates place`, `/uxmcrates tool`, a right click with the link tool |
| `uxmcrates.admin.remove` | `/uxmcrates remove`, a left click with the link tool |
| `uxmcrates.admin.forceopen` | `/uxmcrates forceopen` |
| `uxmcrates.admin.log` | `/uxmcrates log` |
| `uxmcrates.admin.migrate` | `/uxmcrates migrate` |
| `uxmcrates.admin.envoy` | `/uxmcrates envoy start`, `stop`, `list` |
| `uxmcrates.admin.key` | `/uxmcrates key` |
| `uxmcrates.admin.key.give` | `key give` |
| `uxmcrates.admin.key.giveitem` | `key giveitem` |
| `uxmcrates.admin.key.take` | `key take` |
| `uxmcrates.admin.key.set` | `key set` |
| `uxmcrates.admin.key.giveall` | `key giveall` |
| `uxmcrates.admin.key.giveoffline` | `key giveoffline` |
| `uxmcrates.admin.key.ledger` | `key ledger` |
| `uxmcrates.admin.reset` | `/uxmcrates reset` |
| `uxmcrates.admin.reset.cooldown` | `reset cooldown` |
| `uxmcrates.admin.reset.player-limit` | `reset player-limit` |
| `uxmcrates.admin.reset.global-limit` | `reset global-limit` |
| `uxmcrates.admin.reroll` | `/uxmcrates reroll` |
| `uxmcrates.admin.reroll.give` | `reroll give` |
| `uxmcrates.admin.language` | `lang set-player`, `lang reset-player` |

All are `op` by default. A give and a take never share a node.

## Nodes you name

These are not declared by the plugin. You write them in your files and grant them with your permission plugin.

| Where | Example | Meaning |
|---|---|---|
| A crate's `permission` | `uxmcrates.crate.vip` | Needed to open the crate, beside its key |
| A reward's `permission` | `rank.vip` | Needed to win the reward; others see it `Locked` |
| `rerolls.groups.<group>.permission` | `uxmcrates.reroll.silver`, `uxmcrates.reroll.gold` | The reroll group a player is on |
| `luck.permission` | `uxmcrates.luck.tier-one` | A weight multiplier |
| A key's `crafting.permission` | `uxmcrates.craft.common` | Needed to craft the key |

## LuckPerms examples

```
lp group default permission set uxmcrates.broadcast.hide false
lp group vip permission set uxmcrates.reroll.silver true
lp group mvp permission set uxmcrates.luck.tier-one true
lp group helper permission set uxmcrates.admin.key.give true
lp group helper permission set uxmcrates.admin.log true
```

Related: [Operator commands](../commands/admin.md), [Rerolls](../features/rerolls-and-milestones.md#rerolls)
