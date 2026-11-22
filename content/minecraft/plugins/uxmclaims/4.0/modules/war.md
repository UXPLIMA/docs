---
title: War
order: 1330
description: Declared wars between enemy lands, towns or nations, with modes, goals, truces and vassals.
icon: swords
---

Enemy lands, towns or nations declare war on each other, prepare, and then fight it out in one of four modes. A
war ends when a side meets the mode's win condition, surrenders, or the time runs out; the war's goal then applies
to the loser and a truce follows. Holders can also opt out of war entirely with a peaceful status.

Module `war` · disabled by default · `modules/war/config.yml` · requires [Relations](relations.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/war`, `/war status` | Shows the wars you fight in. | `uxmclaims.command.war.use` |
| `/war declare <target> [mode] [goal]` | Declares war on an enemy land, town or nation, or sends an offer when mutual acceptance is on. Without a mode and with `pick-in-menu` on, a menu picks mode and goal. | `uxmclaims.command.war.use` + `uxmclaims.ability.war.declare` + role `war:manage` |
| `/war surrender [enemy]` | Gives up a war of what you run. | `uxmclaims.command.war.use` + role `war:manage` |
| `/war accept [attacker]` | Accepts a war offered to what you run. | `uxmclaims.command.war.use` + role `war:manage` |
| `/war deny [attacker]` | Turns down a war offer. | `uxmclaims.command.war.use` + role `war:manage` |
| `/war peaceful [on\|off] [land\|town\|nation]` | Shows or switches the peaceful status of what you run. | `uxmclaims.command.war.use` + role `war:manage` |
| `/war list` | Lists every running war. | `uxmclaims.command.war.use` |
| `/war flag` | Moves your siege flag to where you stand, during the preparation. | `uxmclaims.command.war.use` + role `war:manage` |
| `/uxm war end <side or war id> <attacker\|defender\|draw>` | Ends a war with the result you give. | `uxmclaims.admin.war` |
| `/uxm war release <vassal>` | Frees a vassal from its overlord. | `uxmclaims.admin.war` |
| `/uxm war list` | Lists every running war. | `uxmclaims.admin.war` |

Your side is your land, town or nation of the same kind as the target; both sides are always of one kind. Names
may contain spaces. Without a kind, `/war peaceful on` acts on your nation, else your town, else your land. The
town and nation menus have a wars button.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.war.use` | everyone | The `/war` commands. |
| `uxmclaims.ability.war.declare` | everyone | Declaring wars for what you run. |
| `uxmclaims.admin.war` | op | The `/uxm war` commands; acting for any side. |
| `uxmclaims.bypass.war.commands` | nobody | Using `blocked-commands` in enemy lands during a war. |

| Role permission | Default holder | Allows |
|---|---|---|
| `war:manage` | owner (land owner, mayor) | Declaring, surrendering, answering offers, the peaceful status and the siege flag. A nation leader always may. |

| Cost key | Default | What it pays for |
|---|---|---|
| `war:declare` | `500` | Declaring a war. Set it in `modules/economy/costs.yml`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `levels` | `land`, `town`, `nation` | Which kinds of holder can go to war. |
| `require-enemy` | `true` | The sides must be enemies in [Relations](relations.md). |
| `default-mode` | `siege` | The mode when `/war declare` names none. |
| `default-goal` | `plunder` | The goal when `/war declare` names none. |
| `pick-in-menu` | `true` | `/war declare <target>` without a mode opens the declare menu. |
| `declaration.cooldown` | `24h` | Wait between two declarations of one holder. |
| `declaration.min-online.attacker` | `1` | Attacker members who must be online to declare. |
| `declaration.min-online.defender` | `1` | Defender members who must be online. |
| `declaration.max-active-wars` | `1` | Wars a holder can be in at once, on either side. |
| `declaration.new-holder-shield` | `3d` | A new holder cannot be attacked for this long; `0s` for none. |
| `declaration.loser-shield` | `3d` | A holder that lost cannot be attacked again for this long. |
| `declaration.max-level-difference` | `-1` | Largest level difference between the sides ([Levels](levels.md)); `-1` for any. |
| `declaration.days` | `[]` | Days wars can be declared (`MONDAY` ... `SUNDAY`); empty for every day. |
| `declaration.hours` | `''` | Hours of the day in server time, such as `18-23`; empty for any. |
| `declaration.mutual.enabled` | `false` | `/war declare` sends an offer the defenders accept or deny. |
| `declaration.mutual.timeout` | `24h` | How long an offer waits. Open offers survive a restart. |
| `peaceful.enabled` | `true` | `false`: nobody is peaceful; stored statuses are kept. |
| `peaceful.switch-cooldown` | `3d` | Wait before a holder may switch its status again. |
| `preparation` | `10m` | Time between declaration and fighting. |
| `duration` | `1h` | Longest fighting time. |
| `on-timeout` | `DEFENDER` | Who wins when time runs out: `DEFENDER`, `SCORE` (more points; equal is a draw) or `DRAW`. |
| `score.per-kill` | `1` | Points per kill for `SCORE`. |
| `score.per-captured-point` | `10` | Points per held capture point for `SCORE`. |
| `truce-after` | `3d` | Truce between the sides after the war; `0s` for none. |
| `in-enemy-land.pvp` | `true` | PvP between the sides in their lands, even where the land's pvp flag is off. |
| `in-enemy-land.allow` | `lands:move_inside`, `lands:use_mechanisms`, ... | Role permission keys fighters get in the other side's lands. |
| `in-enemy-land.build` | `lands:block_break`, `lands:block_place` | Added to `allow` when the mode lets attackers build. |
| `freeze.unclaim` | `true` | No unclaiming of a side's lands during a war. |
| `freeze.members` | `true` | No new members in a side's lands. |
| `freeze.delete` | `true` | No deleting a land that is itself a side. |
| `freeze.relations` | `true` | No ending the enemy relation between the sides. |
| `blocked-commands` | `spawn`, `home`, `tpa`, `warp`, `claim spawn`, ... | Commands a fighter cannot use in the other side's lands during the fighting. |
| `combat-tag` | `true` | Every hit between the sides restarts both combat tags ([Combat](combat.md)). |
| `boss-bar.enabled` | `true` | Shows the capture progress to online fighters. |
| `boss-bar.color` | `RED` | `PINK`, `BLUE`, `RED`, `GREEN`, `YELLOW`, `PURPLE` or `WHITE`. |
| `boss-bar.style` | `SEGMENTED_10` | `SOLID`, `SEGMENTED_6`, `SEGMENTED_10`, `SEGMENTED_12` or `SEGMENTED_20`. |
| `broadcast` | `true` | Tells every online player when a war is declared, starts and ends; else only the fighters. |

## Modes

Each mode has `modes.<mode>.enabled` (default `true`). Siege, king of the hill and skirmish never allow block
damage.

| Mode | How it is won | Keys (default) |
|---|---|---|
| `siege` | The defenders' flag stands at their main land's spawn, or the middle of its main chunk. Attackers win by standing in the flag area, with no defender there, for `hold-time`. | `hold-time` (`2m`), `radius` (`8`), `height` (`8`, `-1` for full height), `decay-per-second` (`1.0`) |
| `capture` | Capture points on border chunks of the defenders' lands. Attackers take a point by standing in its chunk, with no defender there, for `hold-time`, and win at `points-to-win`. | `points` (`3`), `points-to-win` (`0` = all), `hold-time` (`1m`), `decay-per-second` (`1.0`), `recapture` (`true`), `allow-build` (`false`) |
| `koth` | One hill in the middle of the defenders' main land. A side alone on it holds it; the first to hold it `hold-time` in total wins, else the side that held it longer. | `hold-time` (`5m`), `radius` (`6`) |
| `skirmish` | No points. The first side to reach `kills-to-win` kills wins, else the side with more kills. | `kills-to-win` (`10`, `0` = time decides) |

`decay-per-second` is the progress lost per second while no attacker stands on the point. With `recapture`,
defenders alone on a taken point take it back.

## Goals

Each goal has `goals.<goal>.enabled` (default `true`).

| Goal | What winning does | Keys (default) |
|---|---|---|
| `plunder` | The winner takes a share of the loser's bank ([Bank](bank.md)). | `percent` (`50.0`), `surrender-percent` (`25.0`), `max-amount` (`0.0` = no cap) |
| `annex` | When the attackers win, defender lands change owner to them, lands with taken points first. | `max-lands` (`1`), `keep-home` (`true`: never a town's home land or a capital's main land) |
| `raid` | Attackers may use the defenders' lands with the `allow` keys during the war, and the winners in the losers' lands for `after-win`. | `after-win` (`1h`), `during-war` (`true`), `allow` (containers, mechanisms, redstone, pickup, break, place) |
| `vassal` | The loser serves the winner for `duration`. | `levels` (`nation`), `duration` (`7d`, `0s` = until released), `tribute-percent` (`5.0`), `tribute-max` (`0.0`), `ally` (`true`) |

In a siege, building and breaking are taken out of the raid keys.

## Vassals and peaceful holders

A vassal pays `tribute-percent` of its bank to its overlord's bank every day. With `ally` the two become allies
instead of getting a truce, and the vassal cannot end that alliance or attack its overlord. A vassal has one
overlord: a new vassal war replaces it, and an overlord beaten by its own vassal loses it. Vassalage ends after
`duration`, with `/uxm war release`, or when either side is deleted. The taxes of the [Taxes](taxes.md) module are
not changed.

A peaceful holder can neither declare war nor be attacked. It cannot switch while at war, and waits
`switch-cooldown` between switches.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_war_at_war%` | `true` or `false`. |
| `%uxmclaims_war_count%` | Running wars the player fights in. |
| `%uxmclaims_war_running%` | Running wars on the server. |
| `%uxmclaims_war_enemy%` | The other side's name. |
| `%uxmclaims_war_side%` | Attacker or Defender. |
| `%uxmclaims_war_state%` | Preparing, Fighting or Over. |
| `%uxmclaims_war_mode%` | The mode's name. |
| `%uxmclaims_war_goal%` | The goal's name. |
| `%uxmclaims_war_time_left%` | Time left in the current phase. |
| `%uxmclaims_war_points%` | Captured points out of the total, such as `1/3`. |
| `%uxmclaims_war_progress%` | Capture progress in percent. |
| `%uxmclaims_war_kills%` | The player's war kills. |
| `%uxmclaims_war_deaths%` | The player's war deaths. |
| `%uxmclaims_war_kdr%` | Kills per death, two decimals. |

The war placeholders describe the player's first running war and are empty when there is none.

## Notes

- With [Rollback](rollback.md) running, every defender land is snapshotted before the fighting starts, so you can undo war damage.
- A level reward `war-shield` in [Levels](levels.md) protects a holder for that many hours after it reached the level; it only works with levels bought in purchase mode.
- The siege flag is an area only; no banner is placed. Capture points are picked at declaration and do not follow later claims.
- `annex` hands over whole lands, not single chunks. A town or nation deleting its own lands is not held back during a war, only a land that is itself a side.
- In network mode vassals, peaceful statuses and offers made on another server are only seen after a restart, and tribute runs on the leading server.
- Other modules and add-ons can register their own modes and goals through the API.

Related: [Relations](relations.md), [Towns](towns.md), [Nations](nations.md), [Bank](bank.md), [Combat](combat.md), [Rollback](rollback.md), [Leaderboards](leaderboards.md)
