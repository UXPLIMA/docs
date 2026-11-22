---
title: Ruins
order: 1332
description: Inactive lands and towns fall into ruin, can be reclaimed for a while, and are then deleted.
icon: landmark
---

When every player of a personal land or a town stays offline long enough, the land or town falls into ruin. A
ruin keeps its chunks with lowered protection, its former members can take it over with a reclaim, and it is
deleted when its time is over. Unpaid upkeep can also ruin a holder, through the [Taxes](taxes.md) module.

Module `ruins` · disabled by default · `modules/ruins/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim reclaim [land]` | Takes over a ruined land you were the owner or a member of. Without a name, the land you stand in. | none |
| `/town reclaim [town]` | Takes over a ruined town you lived in. Without a name, your own town. | none |
| `/uxm ruins list` | Lists the ruins, the check mode, and up to 20 holders the last check found inactive. | `uxmclaims.command.ruins.admin` |
| `/uxm ruins run` | Runs the inactivity and run-out check now. | `uxmclaims.command.ruins.admin` |
| `/uxm ruins ruin <land\|town> <name>` | Turns a land or town into a ruin. | `uxmclaims.command.ruins.admin` |
| `/uxm ruins restore <land\|town> <name>` | Ends a ruin without changing owners. | `uxmclaims.command.ruins.admin` |

A reclaim ends the ruin and hands the land, or the mayorship of the town, to you.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.ruins.admin` | op | The `/uxm ruins` commands. |

| Cost key | Default | What it pays for |
|---|---|---|
| `ruins:reclaim` | `0` | One reclaim. Set the price in `modules/economy/costs.yml`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `status` | `preview` | The inactivity check: `off` never ruins for inactivity, `preview` only lists and logs what it would ruin, `active` ruins. Taxes and admins can ruin in every mode. |
| `check-every` | `1h` | How often inactivity and ruins whose time is over are checked. At least one minute. |
| `inactive-after` | `30d` | A personal land is inactive when its owner and members, a town when all its residents, have been offline this long. |
| `inactivity.lands` | `true` | The check looks at personal lands. |
| `inactivity.towns` | `true` | The check looks at towns. |
| `ruin.duration` | `72h` | How long a holder stays a ruin before `end-action`. |
| `ruin.end-action` | `delete` | `delete` deletes the land or town when the time is over; `keep` leaves it a ruin until reclaimed or restored. |
| `ruin.reclaim-after` | `4h` | Former members (owner, members, residents) may reclaim after this wait. |
| `ruin.open-reclaim` | `false` | Whether anyone may reclaim after `open-after`. |
| `ruin.open-after` | `48h` | When anyone may reclaim, counted from the moment it fell into ruin. |
| `ruin.restore-on-return` | `false` | Ends the ruin for free when its owner or mayor joins again. |
| `ruin.protection.pvp` | `true` | Players may fight inside a ruin. |
| `ruin.protection.build` | `false` | Anyone may break and place blocks and use buckets. |
| `ruin.protection.interact` | `false` | Anyone may open doors and containers and use entities. |
| `ruin.protection.environment` | `true` | Fire spread, pistons, fluids and growth cross into the ruin. |
| `ruin.protection.enter` | `true` | Anyone may walk or teleport in. Land bans still apply. |

For each `ruin.protection` key, `true` lets everyone do it and `false` keeps the land's own rules.

## Notes

- The check starts in `preview`. Run `/uxm ruins list` to see what it would ruin before you switch to `active`.
- Server lands are never ruined, and a holder whose owner or residents have no known profile is skipped.
- In network mode only the leading server runs the check.
- A reclaim is two writes: the paid end of the ruin, then the hand-over. If the hand-over fails, it is only logged.
- Inactivity is judged by last seen time only. There are no playtime tiers, no warning before a ruin, and no sell or transfer outcome for personal lands yet. A town whose mayor leaves with no successor is not ruined.
- A ruined holder is still charged by [Taxes](taxes.md), and the bankruptcy notice repeats every day it stays at the debt cap.

Related: [Lands](lands.md), [Towns](towns.md), [Taxes](taxes.md), [Expiry](expiry.md)
