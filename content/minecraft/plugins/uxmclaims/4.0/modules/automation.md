---
title: Automation
order: 1352
description: No-code rules that run commands, messages or money changes when an uxmClaims event happens.
icon: workflow
---

The automation module runs rules you write in `rules.yml`: when an uxmClaims event happens and the rule's
conditions hold, it runs commands, sends messages, moves money, or cancels the event. There is no scripting
engine; conditions and actions come from a fixed list.

Module `automation` · disabled by default · `modules/automation/config.yml`

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm automation` | Lists the switched-on rules, their event, how many actions each has and how often it ran, plus rules skipped for errors. | `uxmclaims.admin.automation` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.automation` | op | `/uxm automation` |

## Settings

| Key | Default | What it does |
|---|---|---|
| `debug` | `false` | Logs every rule that runs, with the event. Useful while setting rules up. |
| `max-actions-per-event` | `20` | The most actions one event may run across all rules. |
| `allow-console-commands` | `true` | Lets rules run `console:` commands. |
| `allow-money` | `true` | Lets rules give or take money with `money:`. |

## Writing a rule

Rules live in `modules/automation/rules.yml`. `/uxm reload` reads them again, and a rule with an error is
skipped and logged.

```yaml
rules:
  big-land-reward:
    enabled: true
    on: LandChunkClaimEvent
    if:
      - "{land.chunks} >= 50"
    if-any: []
    do:
      - "console: give {target} diamond 1"
      - "message: &aThanks for building &f{land.name}&a!"
    target: auto
    cooldown: 1d
```

| Key | What it does |
|---|---|
| `enabled` | Switches the rule on. Both shipped example rules are off. |
| `on` | The event's class name, such as `TownJoinEvent` or `PreLandChunkClaimEvent`. A group name (`TownEvent`, `LandEvent`) matches every event of the group. Several names: `"TownJoinEvent, TownLeaveEvent"`. |
| `if` | Conditions that must all hold. |
| `if-any` | When given, at least one of these must hold too. |
| `do` | The actions, in order. |
| `target` | Whose player actions these are: `auto` (the event's player, else the actor), `actor`, or the name of a field holding a player id. |
| `cooldown` | Least time between two runs for one target, such as `30s` or `5m`. |

| Action | What it does |
|---|---|
| `console: <command>` | Runs a command as the console. |
| `player: <command>` | Makes the target player run a command. |
| `message: <text>` | Sends the target a chat message. |
| `actionbar: <text>` | Shows the target an action bar. |
| `broadcast: <text>` | Sends a message to every player and the console. |
| `money: <amount>` | Gives the target money; a negative amount takes it. Needs [Economy](economy.md). |
| `log: <text>` | Writes a line to the server log. |
| `cancel` or `cancel: <reason>` | Cancels the event. Works only on `Pre` events. |

A condition is `<left> <op> <right>` with `==`, `!=`, `>`, `<`, `>=`, `<=`, `contains`, `starts-with` or
`ends-with`. `permission <node>` and `!permission <node>` ask the target player. A lone value is true when it
reads `true`. Numbers compare as numbers, a list reads as its size (`{land.chunks}`), and a time span as
seconds.

In conditions and actions, `{field.path}` reads the event, such as `{town.name}`, `{land.name}`, `{actor.name}`,
`{player.name}` or `{chunk.x}`. `{event}`, `{target}` and `{target-uuid}` are always there. `%placeholders%` go
through PlaceholderAPI for the target player when it is installed.

## Notes

- Texts use `&` colour codes and `&#rrggbb`, not MiniMessage.
- Cooldowns are kept in memory, so a restart clears them.
- Only uxmClaims events can start a rule, not other Bukkit events. There is no arithmetic and no functions in
  conditions, and no recipient groups such as "every online town member".
- A `cancel` on an event that cannot be cancelled is ignored and logged once.

Related: [Audit Log](audit-log.md), [Webhooks](webhooks.md), [Events](../developer/events.md)
