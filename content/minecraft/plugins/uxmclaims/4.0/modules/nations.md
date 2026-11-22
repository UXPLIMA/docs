---
title: Nations
order: 1321
description: Groups of towns with a capital, a leader, nation ranks and a spawn.
icon: flag
---

A nation is a group of towns led from a capital town. The capital's mayor is the nation's leader, and every
resident of a member town is a citizen. Mayors found, join and leave nations for their town with `/nation`,
which opens the nation menu once the town belongs to one.

Module `nations` · disabled by default · `modules/nations/config.yml` · requires [Lands](lands.md), [Towns](towns.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/nation` | Opens your nation menu, or shows how to found a nation. | `uxmclaims.command.nations.menu` |
| `/nation menu` | Opens your nation menu. | `uxmclaims.command.nations.menu` |
| `/nation new <name>` | Founds a nation with your town as its capital. Alias `create`. | `uxmclaims.command.nations.new` |
| `/nation delete` | Deletes your nation, after confirmation (leader only). Its towns stay. | `uxmclaims.command.nations.delete` |
| `/nation confirm` | Confirms the delete. | `uxmclaims.command.nations.menu` |
| `/nation info [nation]` | Shows a nation, yours by default. | `uxmclaims.command.nations.info` |
| `/nation list` | Lists every nation. | `uxmclaims.command.nations.list` |
| `/nation spawn [nation]` | Teleports to your nation's spawn. | `uxmclaims.command.nations.spawn` |
| `/nation invite <town>` | Invites a town. | `uxmclaims.command.nations.invite` |
| `/nation kick <town>` | Removes a member town. | `uxmclaims.command.nations.kick` |
| `/nation join <nation>` | Brings your town into an open nation, or one that invited it. | `uxmclaims.command.nations.join` |
| `/nation accept [nation]` | Accepts a nation invite for your town. | `uxmclaims.command.nations.join` |
| `/nation deny [nation]` | Declines a nation invite for your town. | `uxmclaims.command.nations.join` |
| `/nation leave` | Takes your town out of its nation. | `uxmclaims.command.nations.leave` |
| `/nation set capital <town>` | Makes a member town the capital (leader only). | `uxmclaims.command.nations.set` |
| `/nation set spawn` | Sets the spawn where you stand, on a land of a member town. | `uxmclaims.command.nations.set` |
| `/nation set name <name>` | Renames the nation. Also `/nation rename`. | `uxmclaims.command.nations.set` |
| `/nation set tag [tag]` | Sets the tag; leave it out to clear it. | `uxmclaims.command.nations.set` |
| `/nation set board [text]` | Sets the board; leave it out to clear it. | `uxmclaims.command.nations.set` |
| `/nation set open [on\|off]` | Lets any town join without an invite. | `uxmclaims.command.nations.set` |
| `/nation toggle open` | Switches open on or off. | `uxmclaims.command.nations.set` |
| `/nation rank list` | Lists the nation's ranks. | `uxmclaims.command.nations.rank` |
| `/nation rank set <player> <rank>` | Gives a citizen a nation rank. Alias `rank add`. | `uxmclaims.command.nations.rank` |
| `/nation rank remove <player>` | Puts a citizen back to the citizen rank. | `uxmclaims.command.nations.rank` |
| `/nation rank create <name> [priority]` | Creates a nation rank. | `uxmclaims.command.nations.rank` |
| `/nation rank delete <name>` | Deletes a custom nation rank. | `uxmclaims.command.nations.rank` |
| `/nation rank permission <rank> <key> <value>` | Allows, denies or unsets a key for a nation rank. | `uxmclaims.command.nations.rank` |
| `/uxm nation info <nation>` | Shows any nation. | `uxmclaims.admin.nations` |
| `/uxm nation delete <nation>` | Deletes any nation. | `uxmclaims.admin.nations` |
| `/uxm nation setcapital <nation> <town>` | Makes a member town the capital. | `uxmclaims.admin.nations` |
| `/uxm nation addtown <nation> <town>` | Puts a town into a nation. | `uxmclaims.admin.nations` |
| `/uxm nation kick <nation> <town>` | Removes a town from a nation. | `uxmclaims.admin.nations` |
| `/uxm nation tp <nation>` | Teleports to any nation's spawn, without warmup. | `uxmclaims.admin.nations` |

`/n` is registered as a short form of `/nation` when `commands.short-alias` is on and no other plugin owns `/n`.
Joining, accepting, declining and leaving act for the town whose mayor you are. Inviting, kicking and the
`set` and `rank` commands also need the nation rank key and ability node below.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.nations.<sub>` | everyone | The `/nation` subcommand, for `menu`, `new`, `delete`, `info`, `list`, `spawn`, `invite`, `kick`, `join`, `leave`, `rank`, `set`. |
| `uxmclaims.ability.nation.create` | everyone | Founding a nation. |
| `uxmclaims.ability.nation.delete` | everyone | Deleting your nation. |
| `uxmclaims.ability.nation.invite` | everyone | Inviting towns. |
| `uxmclaims.ability.nation.kick` | everyone | Kicking towns. |
| `uxmclaims.ability.nation.rank` | everyone | Giving ranks and creating, deleting and editing them. |
| `uxmclaims.ability.nation.set` | everyone | Changing spawn, name, tag, board and open. |
| `uxmclaims.ability.nation.spawn` | everyone | Teleporting to a nation spawn with `/nation spawn`. |
| `uxmclaims.admin.nations` | op | The `/uxm nation` commands, which act on any nation without rank checks, and `/nation spawn <nation>` to any nation. |
| `uxmclaims.bypass.priority` | op | Manages nation ranks and citizens regardless of rank priority (registered by [Lands](lands.md)). |

| Rank key | Allows |
|---|---|
| `nations:invite`, `nations:kick` | Inviting and kicking towns. |
| `nations:rank` | Giving and removing nation ranks. |
| `nations:manage_ranks` | Creating, deleting and editing nation ranks. |
| `nations:set_spawn`, `nations:set_name`, `nations:set_tag`, `nations:set_board` | The matching `/nation set` command. |
| `nations:toggle` | Open. |
| `nations:manage_lands` | Managing lands the nation owns. |
| `lands:*`, `warps:use`, ... | Land role keys, applied to citizens on the lands of other member towns and on nation-owned lands. |

| Cost key | Default | What it pays for |
|---|---|---|
| `nations:create` | `1000` | Founding a nation. |
| `nations:rename` | `0` | Renaming. |
| `nations:spawn` | `0` | Teleporting to the nation spawn. |

Prices are set in [Economy](economy.md) (`modules/economy/costs.yml`).

## Settings

| Key | Default | What it does |
|---|---|---|
| `creation.min-residents` | `3` | Residents the capital town needs to found a nation. |
| `creation.name.min` / `creation.name.max` | `3` / `24` | Name length. |
| `creation.name.pattern` | `[\p{L}\p{N}_\-]+` | Characters a name may use. |
| `creation.tag.max-length` | `4` | Longest tag. |
| `creation.tag.pattern` | `[\p{L}\p{N}]+` | Characters a tag may use. |
| `membership.invite-expiry` | `3d` | How long an invite stays valid; `0` until answered. |
| `proximity.max-distance-from-capital` | `0` | Most chunks between the capital's home land and a member town's; `0` turns it off. While on, towns in another world are refused. |
| `spawn.warmup-seconds` | `3` | Seconds to stand still before `/nation spawn` teleports. |
| `spawn.cancel-on-move` | `true` | Moving cancels the warmup. |
| `spawn.safe` | `true` | Moves the target to safe ground when it is unsafe. |
| `board.max-length` | `170` | Longest board text. |
| `commands.short-alias` | `true` | Registers `/n`. Needs a restart. |
| `map.enabled` | `true` | Draws nations on [Web Maps](webmap.md). |
| `map.label` | `Nations` | Layer name on the map. |
| `map.fill-color` / `map.fill-opacity` | `#4A6FD8` / `0.15` | Fill of nation lands. |
| `map.line-color` / `map.line-opacity` / `map.line-weight` | `#2E4BA8` / `0.9` / `3` | Outline of nation lands. |
| `economy.payer` | `NATION_BANK` | Who pays: `NATION_BANK` (with [Bank](bank.md) on, the founding town's bank pays for founding and the nation's bank for renames) or `PLAYER`. |
| `confirm-seconds` | `30` | How long a `/nation confirm` stays valid. |

## Ranks

`ranks.yml` holds the ranks every new nation starts with; existing nations keep theirs. A rank has a `name`,
`type`, `priority`, `permissions` and `denied`, as in [Towns](towns.md#ranks).

| Rank | Type | Priority | Shipped with |
|---|---|---|---|
| `leader` | `OWNER` | 100 | Everything on nation-owned lands. The capital's mayor. |
| `minister` | `CUSTOM` | 50 | Invite, kick, rank, set board, set spawn, entering, teleporting, mechanisms, `warps:use`. |
| `citizen` | `MEMBER` | 10 | Entering and teleporting on member towns' lands, `warps:use`. Every citizen. |
| `foreigner` | `VISITOR` | 0 | Nothing. A placeholder. |

At home, a resident uses their own town rank; the nation rank applies on the lands of the other member towns.
On a member town's lands the leader is a plain citizen.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_nation_name%` | The player's nation name. |
| `%uxmclaims_nation_tag%` | Its tag, empty when none. |
| `%uxmclaims_nation_leader%` | The leader's name. |
| `%uxmclaims_nation_members%` | Number of member towns. |
| `%uxmclaims_nation_rank%` | The player's nation rank name. |

All render nothing for a player outside a nation.

## Notes

- The capital cannot leave its nation: make another town the capital first with `/nation set capital`.
- When the capital town is deleted, the oldest other member becomes the capital. A nation with no members left is deleted.
- Only citizens reach a nation's spawn; there is no public switch for nations yet. The optional name on `/nation spawn` only works for players with `uxmclaims.admin.nations`.
- With `economy.payer: NATION_BANK`, the bank payment and the nation write are two separate writes. If the server stops between a failed write and its refund, the refund is lost; the bank ledger shows it.
- Nations need towns: personal lands cannot join a nation yet, so a preset that turns nations on leaves them off with a warning on a server without [Towns](towns.md).
- Nation list and member entries in the menus are still in the server language.

Related: [Towns](towns.md), [Relations](relations.md), [Bank](bank.md), [Levels](levels.md), [War](war.md)
