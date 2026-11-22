---
title: Jails
order: 1331
description: Town jail cells, timed sentences with bail, and outlaw lists that keep players out of a town.
icon: lock
---

A town sets jail cells inside its lands and jails its residents or outlaws there for a time, with an optional
bail the inmate can pay to leave early. Inmates cannot teleport or use blocked commands, and see the time left
in a boss bar. Outlaws are kept out of the town's lands.

Module `jails` · disabled by default · `modules/jails/config.yml` · requires [Towns](towns.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town jail <player> [time] [bail] [cell]` | Jails a resident or outlaw of your town. | `uxmclaims.command.jails` + role `jails:jail` |
| `/town unjail <player>` | Lets a player out of your town's jail. | `uxmclaims.command.jails` + role `jails:jail` |
| `/town jails` | Lists your town's cells and inmates in chat. | `uxmclaims.command.jails` |
| `/town jailcell add <name>` | Makes the spot you stand on a cell of your town. | `uxmclaims.command.jails` + role `jails:cells` |
| `/town jailcell remove <name>` | Removes a cell. | `uxmclaims.command.jails` + role `jails:cells` |
| `/town outlaw add <player>` | Makes a player an outlaw of your town. | `uxmclaims.command.jails` + role `jails:outlaw` |
| `/town outlaw remove <player>` | Takes a player off the outlaw list. | `uxmclaims.command.jails` + role `jails:outlaw` |
| `/town outlaws` | Lists your town's outlaws. | `uxmclaims.command.jails` |
| `/bail` | Shows your time left and your bail. | none |
| `/bail pay` | Pays your bail and leaves jail. | none |
| `/uxm jails release <player>` | Lets any player out of any jail. | `uxmclaims.admin.jails` |

`time` takes `30m`, `5h`, `2d`, `1w` or combinations such as `1d12h`; a plain number counts hours. Leave it out for
`sentences.default-duration`. Write `-` as the bail to jail without one while still naming a cell. Without a cell
name the town's first cell is used. Cell names are 1 to 24 letters, digits, `_` or `-`.

The jails button on the town menu shows the same list as `/town jails`.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.jails` | everyone | The `/town` jail, cell and outlaw commands, where the town role allows them. |
| `uxmclaims.admin.jails` | op | Jail and release in any town without role checks or limits; `/uxm jails release`. |
| `uxmclaims.bypass.jail` | nobody | The player cannot be jailed. Offline players are checked through the [LuckPerms](luckperms.md) module; without it they count as not having it. |
| `uxmclaims.bypass.outlaw` | nobody | An outlaw with it may still enter the town. |

| Role permission | Default holder | Allows |
|---|---|---|
| `jails:jail` | mayor | Jailing and releasing. |
| `jails:outlaw` | mayor | Changing the outlaw list. |
| `jails:cells` | mayor | Adding and removing cells. |

| Cost key | Default | What it pays for |
|---|---|---|
| `jails:bail` | the bail set by the town | Charged by `/bail pay`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `sentences.max-duration` | `3d` | The longest sentence a town may give. |
| `sentences.default-duration` | `1h` | The sentence when none is given. |
| `sentences.max-inmates-per-town` | `0` | The most players one town may hold at once; `0` for no limit. |
| `sentences.newbie-immunity` | `true` | Players under [Newbie Protection](newbie-protection.md) cannot be jailed. |
| `cells.max-per-town` | `5` | The most cells one town may have. |
| `cells.require-jail-plot` | `false` | With [Plots](plots.md) on, a cell must stand in a plot of type `jail`. Admins may place cells anywhere in the town. |
| `bail.enabled` | `true` | Whether towns may set a bail. |
| `bail.max` | `10000` | The highest bail a town may set. |
| `bail.to-town-bank` | `true` | Pay the bail into the town bank when the [Bank](bank.md) module is on; else it only leaves the wallet. |
| `escape.radius` | `12` | How far in blocks an inmate may walk from the cell. |
| `escape.action` | `RETURN` | `RETURN` puts an inmate who got further back in the cell; `FREE` lets them go. |
| `escape.outlaw-on-escape` | `true` | With `FREE`, the escaped player becomes an outlaw of the town. |
| `restrictions.deny-teleport` | `true` | Blocks teleports by commands, plugins and portals for inmates. |
| `restrictions.deny-ender-pearl` | `true` | Blocks ender pearls and chorus fruit for inmates. |
| `restrictions.blocked-commands` | `spawn`, `home`, `tpa`, `warp`, `rtp`, `claim spawn`, `town spawn`, ... | Commands an inmate cannot use. An entry blocks the command and its subcommands. |
| `release-to` | `TOWN_SPAWN` | Where a released player goes: `TOWN_SPAWN`, `WORLD_SPAWN` or `STAY`. |
| `boss-bar` | `true` | Shows the time left in a boss bar while jailed. |
| `outlaws.deny-entry` | `true` | Outlaws cannot walk or teleport into the town's lands, and are put out when found inside. |
| `outlaws.kick-residents` | `true` | Making a resident an outlaw kicks them from the town first. |
| `outlaws.eject-search-radius` | `8` | How far in chunks to look for a spot outside the land when putting an outlaw out. |

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_jails_jailed%` | `true` or `false`. |
| `%uxmclaims_jails_time_left%` | Time left on the sentence; empty when free. |
| `%uxmclaims_jails_town%` | The town that jailed the player; empty when free. |
| `%uxmclaims_jails_bail%` | The bail; empty when free or with no bail. |

## Notes

- The mayor cannot be jailed or made an outlaw, and you cannot jail yourself.
- Sentences run in real time, also while the inmate is offline.
- Deleting a town frees its inmates and removes its cells and outlaw list. Cells left outside the town after an unclaim are not removed.
- With [Plots](plots.md) on, a `jail` plot type is added, but it gets no flags in the plots config by itself.
- Paying a bail into the town bank and the release are two separate writes: if the release fails after the deposit, it is only logged.
- There is no jailing on death and no jail menu yet; Bedrock players use the same chat commands.

Related: [Towns](towns.md), [Plots](plots.md), [Bank](bank.md), [Newbie Protection](newbie-protection.md), [Random Teleport](rtp.md)
