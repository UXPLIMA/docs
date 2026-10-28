---
title: Boost rewards
order: 35
description: Rewarding a Discord server boost in game, the cooldown, and taking the reward back when the boost ends.
icon: rocket
---

A player who boosts your Discord server types `/boostprize` in game and gets the rewards you set,
again every time the cooldown runs out. When the boost ends, the plugin can take back what it gave.

Everything is in `boost-rewards.yml`, and it ships turned on. Players need
`uxmdiscordsync.boostreward` for the command, which everyone has by default.

```yaml
enabled: true
cooldown: 86400
cooldown-mode: "per-server"
rewards:
  - "give %player% diamond 10"
  - "eco give %player% 1000"
boost-end-commands: []
boost-end-on-leave: true
boost-end-scope: "once"
```

## Claiming

| The player is told | Why |
|---|---|
| The reward was given | Everything below is in order |
| They are not linked | The boost belongs to a Discord account, so the player has to link first |
| Another account gets this boost, and its name | The booster's Discord account was linked to that Minecraft account first. See below |
| They are not boosting | Discord does not show them boosting your server right now |
| To wait, and how long | The cooldown is still running |

`rewards` are console commands. They take `%player%`, `%player_uuid%`, `%discord_id%`,
`%discord_user%` and `%server_name%`, and PlaceholderAPI placeholders. Each claim is written to the
`boost-claim` [audit log](logging.md#audit-log), which is the answer when a reward is disputed.

## One boost, one account

A boost pays one Minecraft account: the first one the booster's Discord account was ever linked to.
A booster who links their Discord account to an alt later cannot claim the same boost twice. If the
move was genuine, `/dsadmin boostowner <player>` hands the boost to the new account, carries the
cooldown along and runs the `boost-end-commands` for the old account.

## The cooldown

`cooldown` is in seconds: `86400` makes it a daily reward, and a very large number a one-time one. A
one-time reward is better given as a permission or rank than as items.

`cooldown-mode` only matters when several servers share the bot:

| Value | Meaning |
|---|---|
| `per-server` | Every server has its own cooldown, so a daily reward can be claimed once a day on each |
| `global` | One cooldown for the whole network |

`multi-server.reward-mode` in `config.yml` makes the same choice. When either says `global`, the
cooldown is global. If your servers share an economy and the reward is money, use `global`, or a
player with five servers claims five times a day.

## When the boost ends

```yaml
boost-end-commands:
  - "lp user %player% parent remove booster"
```

These run when a member stops boosting, and with `boost-end-on-leave: true` also when a boosting
member leaves the Discord server. They run for the Minecraft account that claimed the reward, even if
it has unlinked since, and only if that account claimed something. They take the same placeholders
as `rewards`. An empty list turns this off.

A boost that ended while no server was watching (the bot offline, every server stopped) is noticed
within ten minutes of the next start and handled the same way.

With several servers, `boost-end-scope` decides where the commands run:

| Value | Use it when |
|---|---|
| `once` | The servers share LuckPerms or the economy: one server runs the commands for the network |
| `every-server` | Every server keeps its own ranks or money: each server takes back what it gave |

## Showing boosts

`%discordsync_discord_boosts%` shows how many boosts your Discord server has, in game and in other
plugins; `%total_boosts%` does the same in the bot's status and in custom commands. See
[Placeholders](../placeholders.md).

Related: [Commands](../commands.md#boostowner), [Multi-server](multi-server.md)
