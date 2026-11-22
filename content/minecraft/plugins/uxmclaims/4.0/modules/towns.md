---
title: Towns
order: 1320
description: Towns with a mayor, residents, ranks, a home land, outposts and a spawn.
icon: castle
---

A town owns lands on behalf of its residents: a home land that grows chunk by chunk and outposts further away.
It has one mayor, residents with ranks, a spawn, a tag and a board. Players reach everything through `/town`,
which opens the town menu once they live in one.

Module `towns` · disabled by default · `modules/towns/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/town` | Opens your town menu, or shows how to found a town. | `uxmclaims.command.towns.menu` |
| `/town menu` | Opens your town menu. | `uxmclaims.command.towns.menu` |
| `/town new <name>` | Founds a town; the chunk you stand in becomes its home land. Alias `create`. | `uxmclaims.command.towns.new` |
| `/town delete` | Deletes your town, after confirmation (mayor only). | `uxmclaims.command.towns.delete` |
| `/town confirm` | Confirms a delete, or a mayor's leave. | `uxmclaims.command.towns.menu` |
| `/town info [town]` | Shows a town, yours by default. | `uxmclaims.command.towns.info` |
| `/town board` | Shows your town's board. | `uxmclaims.command.towns.info` |
| `/town list` | Lists every town. | `uxmclaims.command.towns.list` |
| `/town online` | Shows which residents are online. | `uxmclaims.command.towns.online` |
| `/town claim` | Adds the chunk you stand in to the home land. | `uxmclaims.command.towns.claim` |
| `/town outpost` | Claims the chunk you stand in as a new outpost. Also `/town claim outpost`. | `uxmclaims.command.towns.outpost` |
| `/town unclaim` | Removes the chunk you stand in; an outpost's last chunk removes the outpost. | `uxmclaims.command.towns.unclaim` |
| `/town spawn [town]` | Teleports to your town's spawn, or to a public town's. | `uxmclaims.command.towns.spawn` |
| `/town join <town>` | Joins an open town, or one that invited you. | `uxmclaims.command.towns.join` |
| `/town accept [town]` | Accepts a town invite. | `uxmclaims.command.towns.join` |
| `/town deny [town]` | Declines a town invite. | `uxmclaims.command.towns.join` |
| `/town invites` | Lists the invites you have. | `uxmclaims.command.towns.join` |
| `/town leave` | Leaves your town. | `uxmclaims.command.towns.leave` |
| `/town invite <player>` | Invites a player. Alias `add`. | `uxmclaims.command.towns.invite` |
| `/town kick <player>` | Removes a resident. | `uxmclaims.command.towns.kick` |
| `/town mayor <player>` | Hands the town to another resident. Also `/town set mayor`. | `uxmclaims.command.towns.mayor` |
| `/town rank list` | Lists the town's ranks. | `uxmclaims.command.towns.rank` |
| `/town rank set <player> <rank>` | Gives a resident a rank. Alias `rank add`. | `uxmclaims.command.towns.rank` |
| `/town rank remove <player>` | Puts a resident back to the resident rank. | `uxmclaims.command.towns.rank` |
| `/town rank create <name> [priority]` | Creates a rank between resident and mayor. | `uxmclaims.command.towns.rank` |
| `/town rank delete <name>` | Deletes a custom rank. | `uxmclaims.command.towns.rank` |
| `/town rank permission <rank> <key> <value>` | Allows, denies or unsets a key for a rank. | `uxmclaims.command.towns.rank` |
| `/town ranks` | Opens the rank menu. | `uxmclaims.command.towns.rank` |
| `/town set spawn` | Sets the spawn where you stand, inside the home land. | `uxmclaims.command.towns.set` |
| `/town set name <name>` | Renames the town. Also `/town rename`. | `uxmclaims.command.towns.set` |
| `/town set tag [tag]` | Sets the tag; leave it out to clear it. | `uxmclaims.command.towns.set` |
| `/town set board [text]` | Sets the board; leave it out to clear it. | `uxmclaims.command.towns.set` |
| `/town set open [on\|off]` | Lets anyone join without an invite. | `uxmclaims.command.towns.set` |
| `/town set public [on\|off]` | Lets outsiders use the town spawn. | `uxmclaims.command.towns.set` |
| `/town toggle open` | Switches open on or off. | `uxmclaims.command.towns.set` |
| `/town toggle public` | Switches public on or off. | `uxmclaims.command.towns.set` |
| `/uxm town info <town>` | Shows any town. | `uxmclaims.admin.towns` |
| `/uxm town delete <town>` | Deletes any town. | `uxmclaims.admin.towns` |
| `/uxm town setmayor <town> <player>` | Makes a resident the mayor. | `uxmclaims.admin.towns` |
| `/uxm town addmember <town> <player>` | Makes a player a resident. | `uxmclaims.admin.towns` |
| `/uxm town kick <town> <player>` | Removes a resident. | `uxmclaims.admin.towns` |
| `/uxm town tp <town>` | Teleports to any town's spawn, without warmup. | `uxmclaims.admin.towns` |

`/t` is registered as a short form of `/town` when `commands.short-alias` is on and no other plugin owns `/t`.
On/off values accept `on`, `off`, `true`, `false`, `yes`, `no`, `allow` and `deny`; `rank permission` also takes
`unset`. A command node only opens the command: the action also needs the rank key in the table below and the
matching ability node.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.towns.<sub>` | everyone | The `/town` subcommand, for `menu`, `new`, `delete`, `info`, `list`, `online`, `claim`, `unclaim`, `outpost`, `spawn`, `join`, `leave`, `invite`, `kick`, `mayor`, `rank`, `set`. |
| `uxmclaims.ability.town.create` | everyone | Founding a town. |
| `uxmclaims.ability.town.delete` | everyone | Deleting your town. |
| `uxmclaims.ability.town.claim` | everyone | Claiming and unclaiming town chunks. |
| `uxmclaims.ability.town.outpost` | everyone | Claiming outposts. |
| `uxmclaims.ability.town.invite` | everyone | Inviting players. |
| `uxmclaims.ability.town.kick` | everyone | Kicking residents. |
| `uxmclaims.ability.town.rank` | everyone | Giving ranks and creating, deleting and editing them. |
| `uxmclaims.ability.town.set` | everyone | Changing spawn, name, tag, board, open and public. |
| `uxmclaims.ability.town.spawn` | everyone | Registered, but not checked yet. |
| `uxmclaims.admin.towns` | op | The `/uxm town` commands, which act on any town without rank checks. |
| `uxmclaims.bypass.priority` | op | Manages ranks and residents regardless of rank priority (registered by [Lands](lands.md)). |

| Rank key | Allows |
|---|---|
| `towns:invite`, `towns:kick` | Inviting and kicking residents. |
| `towns:claim`, `towns:unclaim`, `towns:outpost` | Claiming, unclaiming, outposts. |
| `towns:rank` | Giving and removing ranks. |
| `towns:manage_ranks` | Creating, deleting and editing ranks. |
| `towns:set_spawn`, `towns:set_name`, `towns:set_tag`, `towns:set_board` | The matching `/town set` command. |
| `towns:toggle` | Open and public. |
| `towns:manage_lands` | Managing the town's lands through `/claim` as their owner would: flags, roles, deleting outposts. |
| `lands:*`, `warps:use`, ... | Any land role key, applied on every land the town owns. |

| Limit | Default | Strategy | What it caps |
|---|---|---|---|
| `uxmclaims.limit.town-chunk` | `16` | stack | Chunks a town may hold, read from the player who claims; `claims.ratio-per-resident` adds to it. |
| `uxmclaims.limit.outpost` | `1` | max | Outposts a town may hold, read from the player who claims. |

| Cost key | Default | What it pays for |
|---|---|---|
| `towns:create` | `250` | Founding a town, always paid by the founder. |
| `towns:claim` | `25` | One town chunk. |
| `towns:outpost` | `500` | One outpost. |
| `towns:rename` | `0` | Renaming. |
| `towns:spawn` | `0` | Teleporting to your own town's spawn. |
| `towns:spawn-public` | `10` | Teleporting to another, public town's spawn. |

Limits are tuned in [Limits](limits.md), prices in [Economy](economy.md) (`modules/economy/costs.yml`).

## Settings

| Key | Default | What it does |
|---|---|---|
| `creation.min-distance-from-other-towns` | `5` | Chunks between a new town or claim and another town's lands; `0` turns it off. |
| `creation.name.min` / `creation.name.max` | `3` / `24` | Name length. |
| `creation.name.pattern` | `[\p{L}\p{N}_\-]+` | Characters a name may use. |
| `creation.tag.max-length` | `4` | Longest tag. |
| `creation.tag.pattern` | `[\p{L}\p{N}]+` | Characters a tag may use. |
| `membership.max-towns-per-player` | `1` | Towns one player may live in at once. |
| `membership.join-cooldown` | `24h` | Wait after leaving a town before joining another; `0` turns it off. |
| `membership.leave-cooldown` | `24h` | Wait after joining before the player may leave; `0` turns it off. |
| `membership.invite-expiry` | `3d` | How long an invite stays valid; `0` until answered. |
| `claims.ratio-per-resident` | `8` | Extra chunks per resident on top of the claimer's limit; `0` turns it off. |
| `outposts.enabled` | `true` | Whether towns may claim outposts. |
| `outposts.min-distance-from-home` | `5` | Chunks an outpost keeps from the home land. |
| `spawn.public-by-default` | `false` | Whether a new town starts public. |
| `spawn.warmup-seconds` | `3` | Seconds to stand still before `/town spawn` teleports. |
| `spawn.cancel-on-move` | `true` | Moving cancels the warmup. |
| `spawn.safe` | `true` | Moves the target to safe ground in its column when it is unsafe. |
| `board.max-length` | `170` | Longest board text. |
| `board.show-on-login` | `true` | Shows the board to residents when they join the server. |
| `deletion.lands` | `DELETE` | `DELETE` removes the town's lands with it; `TRANSFER_TO_MAYOR` gives them to the last mayor as personal lands. |
| `deletion.delete-when-empty` | `true` | Deletes the town when its last resident leaves; with `false` the last resident cannot leave. |
| `commands.short-alias` | `true` | Registers `/t`. Needs a restart. |
| `map.enabled` | `true` | Draws towns on [Web Maps](webmap.md). |
| `map.label` | `Towns` | Layer name on the map. |
| `map.fill-color` / `map.fill-opacity` | `#3BA55D` / `0.25` | Fill of town lands. |
| `map.line-color` / `map.line-opacity` / `map.line-weight` | `#2D7D46` / `0.9` / `2` | Outline of town lands. |
| `map.outpost-color` | `#E0A030` | Fill of outposts. |
| `economy.payer` | `TOWN_BANK` | Who pays for claims, outposts and renames: `TOWN_BANK` (the town's bank when [Bank](bank.md) is on, else the player) or `PLAYER`. |
| `confirm-seconds` | `30` | How long a `/town confirm` stays valid. |

## Ranks

`ranks.yml` next to `config.yml` holds the ranks every new town starts with; existing towns keep theirs and
change them with `/town rank` or the rank menu. Each rank has a `name`, a `type`, a `priority` and lists of
`permissions` and `denied` keys. Keys a rank does not list fall through to the land's visitor role.

| Rank | Type | Priority | Shipped with |
|---|---|---|---|
| `mayor` | `OWNER` | 100 | Everything. Exactly one. |
| `assistant` | `CUSTOM` | 50 | Invite, kick, claim, unclaim, outpost, rank, set spawn, set board, toggle, and building and using land keys. |
| `resident` | `MEMBER` | 10 | Building and using land keys and `warps:use`. New residents get it. Exactly one. |
| `outsider` | `VISITOR` | 0 | Nothing. A placeholder; outsiders follow each land's own visitor role. Exactly one. |

Nobody manages a rank or a resident at or above their own priority.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_town_name%` | The player's town name. |
| `%uxmclaims_town_tag%` | Its tag, empty when none. |
| `%uxmclaims_town_mayor%` | The mayor's name. |
| `%uxmclaims_town_residents%` | Number of residents. |
| `%uxmclaims_town_lands%` | Number of lands (home land and outposts). |
| `%uxmclaims_town_rank%` | The player's rank name. |

All render nothing for a player without a town.

## Notes

- A town claim must touch the town and stay in the home land's world; use `/town outpost` for land further away or in another world.
- When the mayor leaves or is removed, the highest-ranked resident takes over. A mayor leaving asks for `/town confirm` first; the last resident leaving deletes the town.
- Taxes and upkeep are not part of this module: see [Taxes](taxes.md). The town bank comes from [Bank](bank.md).
- With `economy.payer: TOWN_BANK`, the bank payment and the town write are two separate writes. If the server stops between a failed write and its refund, the refund is lost; the bank ledger shows it.
- Town list entries and resident lists in the menus are still in the server language, not each viewer's.
- Bedrock players get confirmations as a form, but the form text still says "Type /town confirm".

Related: [Lands](lands.md), [Nations](nations.md), [Bank](bank.md), [Taxes](taxes.md), [Plots](plots.md), [Levels](levels.md), [Relations](relations.md)
