---
title: Relations
order: 1322
description: Alliances, enemies and truces between lands, towns and nations, and what each side may do on the other's land.
icon: handshake
---

The relations module lets the people who run a land, a town or a nation declare another one an ally, an enemy
or a partner in a truce. A relation changes what the other side's players may do on your land, and allies do
not hurt each other in PvP. The same commands sit under `/claim`, `/town` and `/nation`.

Module `relations` · disabled by default · `modules/relations/config.yml` · requires [Lands](lands.md)

## Commands

Every command below exists in three forms: `/claim ...` for the land you stand in, `/town ...` for your town
and `/nation ...` for your nation. The `/town` forms need [Towns](towns.md), the `/nation` forms need
[Nations](nations.md), and the `/claim` forms need `land` in `levels`.

| Command | What it does | Permission |
|---|---|---|
| `/claim relations` | Opens the relations menu. | `uxmclaims.command.relations.use` |
| `/claim ally list` | Opens the same menu: relations and open proposals. | `uxmclaims.command.relations.use` |
| `/claim ally add <name>` | Proposes an alliance, or accepts the one they proposed to you. | `uxmclaims.command.relations.use` and `uxmclaims.ability.relation.ally` |
| `/claim ally accept <name>` | Accepts their proposed alliance. | `uxmclaims.command.relations.use` |
| `/claim ally deny <name>` | Turns down every open proposal from them. | `uxmclaims.command.relations.use` |
| `/claim ally remove <name>` | Ends the relation with them (same as `neutral`). | `uxmclaims.command.relations.use` |
| `/claim enemy add <name>` | Declares them an enemy. | `uxmclaims.command.relations.use` and `uxmclaims.ability.relation.enemy` |
| `/claim enemy remove <name>` | Takes back your enemy declaration. | `uxmclaims.command.relations.use` |
| `/claim neutral <name>` | Drops any relation you set with them. | `uxmclaims.command.relations.use` |
| `/claim truce propose <name> [time]` | Proposes a truce for a time such as `12h` or `3d`, or accepts theirs. | `uxmclaims.command.relations.use` and `uxmclaims.ability.relation.truce` |
| `/claim truce accept <name>` | Accepts their proposed truce. | `uxmclaims.command.relations.use` |
| `/claim relation role <ally\|truce\|enemy\|nation> <permission> <allow\|deny\|unset>` | Sets what players of a related holder may do on your land. | `uxmclaims.command.relations.use` |
| `/uxm relations set <land\|town\|nation> <from> <to> <ally\|truce\|enemy\|neutral>` | Sets a relation between two holders directly, without a proposal. | `uxmclaims.admin.relations` |

Only someone who runs the holder may use these: the land's owner (or a member whose role allows
`relations:manage`), the mayor or a town rank with `relations:manage`, the nation leader or a nation rank with
it. Names with spaces work as typed. A truce time takes `s`, `m`, `h`, `d` or `w`; with no time,
`truce.default-duration` is used.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.relations.use` | everyone | The relation commands under `/claim`, `/town` and `/nation`. |
| `uxmclaims.ability.relation.ally` | everyone | Proposing alliances. |
| `uxmclaims.ability.relation.enemy` | everyone | Declaring enemies. |
| `uxmclaims.ability.relation.truce` | everyone | Proposing truces. |
| `uxmclaims.admin.relations` | op | Running every holder's relations, skipping the limits, and `/uxm relations set`. |

| Role permission | Given to | Grants |
|---|---|---|
| `relations:manage` | owner | Changing the relations of the land, town or nation. |

| Limit | Default | Counts |
|---|---|---|
| `uxmclaims.limit.ally.<n>` | `ally.max` (5) | Allies per holder. |
| `uxmclaims.limit.enemy.<n>` | `enemy.max` (10) | Enemies per holder. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `levels` | `[land, town, nation]` | Which holders can have relations. Remove `land` on a server where only towns and nations do diplomacy. |
| `precedence` | `TOWN_FIRST` | Which relation counts when a player and a land are related on several levels. `TOWN_FIRST`: land, then town, then nation. `NATION_FIRST`: nation, then town, then land. |
| `ally.require-acceptance` | `true` | The other side has to accept an alliance. With `false`, `ally add` makes the alliance at once. |
| `ally.max` | `5` | Most allies per holder when no limit node sets one. |
| `truce.enabled` | `true` | Whether truces can be proposed. |
| `truce.default-duration` | `3d` | Length of a truce proposed without a time. |
| `truce.max-duration` | `30d` | Longest truce that can be proposed. |
| `enemy.notify-target` | `true` | Tell the other side when it is declared an enemy. |
| `enemy.requires-mutual` | `false` | `true`: two holders are enemies only when both declared it. `false`: one side is enough. |
| `enemy.max` | `10` | Most enemies per holder when no limit node sets one. |
| `ally-friendly-fire` | `false` | Whether allies, and members of one nation, can hurt each other in PvP. |
| `request-expiry` | `3d` | How long an alliance or truce proposal waits for an answer. |
| `roles.<relation>.allow` | see file | Role permission keys players of a related holder get on your lands. `<relation>` is `nation`, `ally`, `truce` or `enemy`. |
| `roles.<relation>.deny` | see file | Role permission keys they are refused. Keys in neither list follow the visitor role. |

## Relation roles

`roles` in the file sets the server default for each relation. The `nation` role covers towns of the same
nation. Out of the box, nation members and allies may enter, teleport, use doors and buttons, trade with
villagers and use warps; truce partners may only enter; enemies may not teleport, use warps, throw ender
pearls or eat chorus fruit.

An owner changes these for their own holder with `relation role`. The permission is a role permission key such
as `lands:block_break` or `warps:use`; a key without a module part is read as `lands:`. `unset` returns the key
to the server default. A land owned by a town or nation follows its owner's relation roles, so change them
with `/town relation role` or `/nation relation role`.

## Notes

- `/claim` forms act on the land you are standing in, not on a land you picked elsewhere.
- You cannot declare an enemy while a truce with them runs. A truce turns neutral by itself when its time is
  up, also while the server was offline.
- `ally deny` drops every open proposal from that holder, a truce proposal included.
- Proposals and declarations reach the people who run the other holder, through the [Inbox](inbox.md) when it
  is on.
- Friendly fire is blocked for direct hits and projectiles between players only.
- When a land, town or nation is deleted, its relations go with it. A land brought back with the lands
  module's restore does not get its relations back.

Related: [Towns](towns.md), [Nations](nations.md), [War](war.md), [Inbox](inbox.md)
