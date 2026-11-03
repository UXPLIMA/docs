---
title: Reference
order: 601
description: Every permission node of uxmSkills with its default and what it allows.
icon: list-checks
---

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. An operator node hides its command.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmskills.use` | true | The windows and your own skills |
| `uxmskills.top` | true | The leaderboards |
| `uxmskills.language` | true | `/uxmskills lang`, `lang set`, `lang reset` |
| `uxmskills.skill.<id>` | true for the shipped eleven | Earning in that skill |
| `uxmskills.hidden` | false | Keeping this account off every board |

A skill you add has an undeclared node, held by operators only until you grant it.

## Nodes you name

| Node | In | Gives |
|---|---|---|
| `uxmskills.multiplier.tier-one`, `tier-two` | `boosts.conf` | 1.25 and 1.5 experience; rename them |
| Any node in `specialisation.nodes` | `config.conf` | How many skills a player may earn in |

## Operators

| Node | Default | Allows |
|---|---|---|
| `uxmskills.admin.doctor` | op | `doctor` |
| `uxmskills.admin.version` | op | `version` and the bare command |
| `uxmskills.admin.reload` | op | `reload`, `curves`, `recalculate` |
| `uxmskills.admin.experience` | op | `give`, `take`, `set`, `setlevel`, `addlevel` |
| `uxmskills.admin.reset` | op | `reset`, `resetall` |
| `uxmskills.admin.wipe` | op | `wipe` |
| `uxmskills.admin.boost` | op | `boost`, `boostall`, `boostoff`, `markitem` |
| `uxmskills.admin.backup` | op | `backup`, `restore` |
| `uxmskills.admin.inspect` | op | `inspect` |
| `uxmskills.admin.diagnose` | op | `diagnose` |
| `uxmskills.admin.import` | op | `import` |
| `uxmskills.admin.stats` | op | `statgive`, `stattake` |
| `uxmskills.admin.edit` | op | `/skillsedit` |
| `uxmskills.admin.language` | op | `lang set-player`, `lang reset-player` |
| `uxmskills.admin.*` | op | Only `admin.version` and `admin.reload`: grant the others one by one, or with your permission plugin's wildcard |

Related: [Commands](../commands/overview.md)
