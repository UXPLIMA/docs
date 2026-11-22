---
title: Lands
order: 1301
description: Claiming chunks, members and roles, land flags, and the world and wilderness rules.
icon: map
---

The lands module is the core of uxmClaims 4.0: a land is a group of chunks with an owner, members, roles and flags. Every other claim module builds on it, and it cannot be turned off. Players start with `/claim` in the chunk they want, and manage the land from `/claim menu`.

Module `lands` · enabled by default (required) · `modules/lands/config.yml`

## Commands

Commands that act on "your land" use, in this order: the land picked with `/claim select`, the land you stand in when you belong to it, or your only land.

| Command | What it does | Permission |
|---|---|---|
| `/claim` | Outside a land: claims the chunk, extending your land next to it or creating a new land after confirmation. Inside a land: opens its menu. | `uxmclaims.command.lands.claim` |
| `/claim create [name]` | Creates a new land at your chunk | `uxmclaims.command.lands.claim` |
| `/claim chunk add` | Adds the chunk you stand in to your land next to it | `uxmclaims.command.lands.claim` |
| `/claim chunk remove` | Removes the chunk you stand in from its land | `uxmclaims.command.lands.unclaim` |
| `/claim unclaim [radius\|all]` | Unclaims this chunk, a square around you, or the whole land (`all` deletes it) | `uxmclaims.command.lands.unclaim` |
| `/claim radius <r>` | Claims every free chunk in a square around you, all or nothing | `uxmclaims.command.lands.radius` |
| `/claim auto` | Claims chunks next to your land as you walk into them; run again to stop | `uxmclaims.command.lands.auto` |
| `/claim fill` | Claims the free chunks your land encloses | `uxmclaims.command.lands.fill` |
| `/claim expand <rows> [direction]` | Claims rows of chunks along one side, the way you look or the one named | `uxmclaims.command.lands.expand` |
| `/claim merge <land>` | Merges another land of yours that touches this one into it | `uxmclaims.command.lands.merge` |
| `/claim select [land]` | Picks the land your commands act on; without a name, a menu to pick or clear it | `uxmclaims.command.lands.select` |
| `/claim menu` | Opens the menu of the land you are in, or the main menu | `uxmclaims.command.lands.menu` |
| `/claim list` | Lists your lands | `uxmclaims.command.lands.list` |
| `/claim map`, `/claim chunk view` | Opens the chunk map around you | `uxmclaims.command.lands.map` |
| `/claim info` | Shows the land you stand in | `uxmclaims.command.lands.info` |
| `/claim flags` | Opens the flag menu | `uxmclaims.command.lands.flag` |
| `/claim setflag <flag> <allow\|deny\|unset>` | Sets a land flag | `uxmclaims.command.lands.flag` |
| `/claim toggleflag <flag>` | Turns a land flag on or off | `uxmclaims.command.lands.flag` |
| `/claim rename <name>` | Renames the land | `uxmclaims.command.lands.rename` |
| `/claim setspawn`, `/claim relocate` | Sets the land spawn where you stand | `uxmclaims.command.lands.setspawn` |
| `/claim spawn [land]`, `/claim home`, `/claim tp` | Teleports to a land spawn | `uxmclaims.command.lands.spawn` |
| `/claim delete [land]` | Deletes a land, after confirmation | `uxmclaims.command.lands.delete` |
| `/claim transfer <player>`, `/claim member transfer` | Gives the land to another player, after confirmation | `uxmclaims.command.lands.transfer` |
| `/claim trust <player> [duration]`, `/claim member add` | Adds a member, for a while if a duration such as `3d` is given | `uxmclaims.command.lands.trust` |
| `/claim untrust <player>`, `/claim kick`, `/claim member kick`, `/claim member remove` | Removes a member | `uxmclaims.command.lands.untrust` |
| `/claim leave [land]`, `/claim member leave` | Leaves a land you are a member of | `uxmclaims.command.lands.leave` |
| `/claim members` | Opens the member menu | `uxmclaims.command.lands.trust` |
| `/claim member setrole <player> <role>` | Gives a member another role | `uxmclaims.command.lands.role` |
| `/claim member promote <player>` | Moves a member one role up | `uxmclaims.command.lands.role` |
| `/claim member demote <player>` | Moves a member one role down | `uxmclaims.command.lands.role` |
| `/claim member setperm <player> <permission> <allow\|deny\|unset>` | Gives or takes one permission from one member | `uxmclaims.command.lands.role` |
| `/claim roles` | Opens the role menu | `uxmclaims.command.lands.role` |
| `/claim role create <name>` | Creates a custom role | `uxmclaims.command.lands.role` |
| `/claim role delete <role>` | Deletes a custom role; its members get the member role | `uxmclaims.command.lands.role` |
| `/claim role rename <role> <name>` | Renames a role | `uxmclaims.command.lands.role` |
| `/claim role setpriority <role> <up\|down>` | Moves a role up or down | `uxmclaims.command.lands.role` |
| `/claim role setperm <role> <permission> <allow\|deny\|unset>` | Sets a role permission | `uxmclaims.command.lands.role` |
| `/claim fly` | Turns land flight on or off | `uxmclaims.command.lands.fly` |
| `/claim unstuck` | Takes you to the nearest unclaimed chunk when you stand in a land you may not be in | `uxmclaims.command.lands.unstuck` |
| `/claim confirm`, `/claim cancel` | Confirms or cancels the pending action | none |

Admin commands. Each `/claim admin ...` command also answers as `/uxm land ...`.

| Command | What it does | Permission |
|---|---|---|
| `/claim admin view [land\|player]`, `/claim admin edit` | Opens the menu of any land, or the lands of a player | `uxmclaims.admin.lands` |
| `/claim admin info [land]` | Shows any land | `uxmclaims.admin.lands` |
| `/claim admin delete [land]` | Deletes any land | `uxmclaims.admin.lands` |
| `/claim admin transfer [land] <player>` | Gives any land to a player; without a land, the one you stand in | `uxmclaims.admin.lands` |
| `/claim admin tp <land>` | Teleports to any land | `uxmclaims.admin.lands` |
| `/claim admin create [name]` | Creates a server-owned admin land at your chunk | `uxmclaims.admin.lands` |
| `/claim admin deleted` | Lists the newest deleted lands that can still be restored | `uxmclaims.admin.lands` |
| `/claim admin restore <land\|id>` | Brings a deleted land back, if its chunks are still free | `uxmclaims.admin.lands` |
| `/claim admin bulk rename <base>` | Renames every land to `<base>-1`, `<base>-2`, ... | `uxmclaims.admin.lands` |
| `/claim admin bulk setflag <flag> <value>` | Sets a flag on every land | `uxmclaims.admin.lands` |
| `/claim admin bulk setrolepermission <member\|visitor> <permission> <value>` | Sets a permission on that built-in role of every land | `uxmclaims.admin.lands` |
| `/claim admin wilderness` | Shows the world and wilderness rules of the world you are in | `uxmclaims.admin.lands` |
| `/claim admin worldrule set <world\|default> <protection\|claimable> <true\|false>` | Changes a world switch and writes it to `worlds.yml` | `uxmclaims.admin.lands` |
| `/claim admin worldrule rule <world\|default> <forced\|wilderness\|claimed-defaults> <key> <ALLOW\|DENY\|UNSET>` | Sets one world rule and writes it to `worlds.yml` | `uxmclaims.admin.lands` |
| `/claim reload` | Reloads config, language and menu files (same as `/uxm reload`) | `uxmclaims.admin` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.lands.<sub>` | everyone | The command of the table above |
| `uxmclaims.ability.claim.create` | everyone | Create a land |
| `uxmclaims.ability.claim.delete` | everyone | Delete a land (also needed to merge) |
| `uxmclaims.ability.claim.rename` | everyone | Rename a land |
| `uxmclaims.ability.claim.relocate` | everyone | Move the land spawn |
| `uxmclaims.ability.claim.transfer` | everyone | Give a land away |
| `uxmclaims.ability.chunk.extend` | everyone | Add chunks (also needed to merge) |
| `uxmclaims.ability.chunk.shrink` | everyone | Remove chunks |
| `uxmclaims.ability.member.invite` | everyone | Add members |
| `uxmclaims.ability.member.kick` | everyone | Remove members |
| `uxmclaims.ability.member.role` | everyone | Change a member's role |
| `uxmclaims.ability.member.permissions.<key>` | everyone | Give a member that single permission |
| `uxmclaims.ability.role.create`, `.delete`, `.rename`, `.priority` | everyone | Manage custom roles |
| `uxmclaims.ability.role.permissions.<key>` | everyone | Change that permission on a role |
| `uxmclaims.ability.claim.flag.<flag>` | everyone | Change that land flag |
| `uxmclaims.bypass.priority` | op | Manage roles, members, town residents and nation ranks whatever your own priority |
| `uxmclaims.bypass.fly` | op | Use `/claim fly` in every land, whatever the role says |
| `uxmclaims.bypass.enter` | op | Enter and stay in lands whose flags or bans keep you out |
| `uxmclaims.bypass.teleport` | op | Teleport into lands (ender pearls, chorus fruit, land spawns) whatever their rules say |
| `uxmclaims.admin.lands` | op | Every admin command above; also skips the teleport warmup |

An ability node is a server-wide switch on top of the land role: a player needs both. Take a node away with your permission plugin to stop a rank from doing that thing on any land.

Limits and delays (see [Limits](limits.md)):

| Node | Default | Strategy | Applies to |
|---|---|---|---|
| `uxmclaims.limit.claim.<n>` | 1 | stack | Lands per player |
| `uxmclaims.limit.chunk.<n>` | 10 | stack | Chunks per land |
| `uxmclaims.limit.member.<n>` | 50 | stack | Members per land |
| `uxmclaims.limit.role.<n>` | 5 | highest | Custom roles per land |
| `uxmclaims.delay.teleport.<n>` | 3 | lowest | Warmup seconds of `/claim spawn` |
| `uxmclaims.delay.rename.<n>` | 30 | lowest | Seconds between land renames |
| `uxmclaims.delay.rolerename.<n>` | 30 | lowest | Seconds between role renames |
| `uxmclaims.delay.relocate.<n>` | 60 | lowest | Seconds between spawn moves |
| `uxmclaims.delay.transfer.<n>` | 300 | lowest | Seconds between transfers |
| `uxmclaims.delay.flags.<n>` | 5 | lowest | Seconds between flag changes |
| `uxmclaims.delay.unstuck.<n>` | 60 | lowest | Seconds between `/claim unstuck` uses |

Costs (prices are set through [Economy](economy.md)):

| Cost key | Default | Charged for |
|---|---|---|
| `lands:land-create` | 0 | Creating a land |
| `lands:chunk-claim` | 25 | Each claimed chunk |
| `lands:rename` | 0 | Renaming a land |
| `lands:role-create` | 0 | Creating a custom role |

### Role permissions

Every land has the roles Owner, Member and Visitor, plus custom roles. Owner may do everything. Visitor is everyone who is not a member and only has `lands:move_inside` by default.

| Key | Allows | Member by default |
|---|---|---|
| `lands:manage_chunks` | Add or remove chunks | no |
| `lands:manage_rename` | Rename the land | no |
| `lands:manage_relocate` | Move the land spawn | no |
| `lands:delete` | Delete the land | no |
| `lands:manage_flags` | Change flags | no |
| `lands:member_permissions` | Give members their own permissions | no |
| `lands:member_role` | Promote and demote members | no |
| `lands:member_trust` | Add players | no |
| `lands:member_kick` | Remove members | no |
| `lands:transfer` | Give the land to another owner | no |
| `lands:manage_roles` | Create, edit and delete roles | no |
| `lands:block_place`, `lands:block_break` | Place and break blocks | yes |
| `lands:sign_edit` | Change sign text | yes |
| `lands:bucket_fill`, `lands:bucket_empty` | Pick up and pour fluids | yes |
| `lands:hanging_place`, `lands:hanging_break` | Hang and take down paintings and item frames | yes |
| `lands:trample_crops` | Trample farmland | no |
| `lands:spawner_place`, `lands:spawner_destroy` | Place and break spawners | no |
| `lands:ignite` | Use flint and steel and fire charges | no |
| `lands:use_redstone` | Buttons, levers, pressure plates | yes |
| `lands:use_mechanisms` | Doors, trapdoors, gates | yes |
| `lands:container_open` | Chests, barrels, furnaces and more | yes |
| `lands:take_lectern_book` | Take a lectern book | no |
| `lands:armor_stand_manipulate` | Change armor stand items | yes |
| `lands:sleep` | Sleep in beds | yes |
| `lands:animal_damage`, `lands:monster_damage` | Hurt animals and monsters | yes |
| `lands:animal_interact` | Feed, breed, leash and tame | yes |
| `lands:shear_entity` | Shear mobs | yes |
| `lands:villager_trade` | Trade with villagers | yes |
| `lands:vehicle_interact` | Use and break minecarts and boats | yes |
| `lands:ride_entity` | Ride mobs | yes |
| `lands:potion_use` | Throw splash and lingering potions | yes |
| `lands:move_inside` | Walk into the land | yes |
| `lands:teleport` | Teleport to the land spawn | yes |
| `lands:enderpearl_use`, `lands:chorus_teleport` | Ender pearls and chorus fruit | yes |
| `lands:elytra_use` | Fly with an elytra | yes |
| `lands:wind_burst`, `lands:wind_charge` | Wind Burst maces and wind charges | yes |
| `lands:item_drop`, `lands:item_pickup` | Drop and pick up items | yes |
| `lands:fishing` | Fish and hook mobs | yes |
| `lands:raid_trigger` | Start a raid with Bad Omen | no |
| `lands:fly` | Fly with `/claim fly` (Owner has it) | no |

Other modules add their own keys: `warps:use`, `warps:manage`, `bans:manage`, `invites:manage`, `expiry:manage`, `vault:access`, `claim-block:manage`, `land-chat:use`, `areas:manage`. Commands accept the 3.x names too, such as `BLOCK_BREAK`.

## Settings

`config.yml`:

| Key | Default | What it does |
|---|---|---|
| `personal-lands.enabled` | `true` | `false` stops new personal lands; existing ones stay |
| `naming.pattern` | `[\p{L}\p{N}_\- ]+` | Characters a land name may use |
| `naming.min-length` | `3` | Shortest land name |
| `naming.max-length` | `100` | Longest land name |
| `naming.uniqueness` | `PER_OWNER` | `PER_OWNER` or `GLOBAL` |
| `claiming.smart-command` | `true` | `/claim` outside a land claims the chunk; `false` opens the menu |
| `claiming.min-distance` | `2` | Chunks that must lie between lands of different owners; `0` lets them touch |
| `claiming.require-adjacent` | `true` | A new chunk must touch the land on a side |
| `claiming.max-radius` | `3` | Largest radius for `/claim radius`, `/claim unclaim <radius>` and rows for `/claim expand` |
| `claiming.auto-enabled` | `true` | Whether `/claim auto` works |
| `claiming.fill-max-chunks` | `64` | Most chunks `/claim fill` adds at once |
| `trust.max-duration` | `30d` | Longest temporary trust; empty means no limit |
| `fly.enabled` | `false` | Turns `/claim fly` on |
| `fly.no-fall-damage` | `true` | No fall damage when land flight ends in the air |
| `list-click` | `MENU` | Click on a land in the list: `MENU`, `TELEPORT` or `NONE` |
| `command-rules.blocked-in-foreign-land` | `[sethome]` | Commands non-members cannot use inside a land |
| `protection.admin-bypass` | `TOGGLE` | `TOGGLE` (`/uxm bypass [on\|off]` with `uxmclaims.admin.bypass`) or `IMPLICIT` (OP and `uxmclaims.admin` always bypass) |
| `confirmation.enabled` | `true` | Ask for `/claim confirm` (a yes/no form on Bedrock) before creating a land with `/claim`, deleting, leaving, merging or transferring a land and deleting a role; `false` runs them at once |
| `confirmation.timeout-seconds` | `30` | How long a confirmation stays open (at least 5) |
| `retention.deleted-days` | `30` | Days a deleted land is kept for `/claim admin restore`; `0` deletes at once |

`flags.yml`:

| Key | Default | What it does |
|---|---|---|
| `defaults` | 13 flags, see below | Flags a new land starts with allowed; all others start denied |
| `admin-only` | `[]` | Flags only admins may change; players see them locked |
| `hidden` | `[]` | Flags the flag menu does not show |

`roles.yml` sets the roles every new land starts with: `defaults.owner`, `defaults.member`, `defaults.visitor` (each with `name`, `permissions` and `denied`) and `custom.<id>` for extra roles, listed highest first. A key in neither list falls back to the visitor role. Existing lands keep their roles.

`worlds.yml` holds `default` (every world) and `worlds.<name>` (overrides only the keys it sets):

| Key | Default | What it does |
|---|---|---|
| `protection` | `true` | `false` turns uxmClaims off in the world |
| `claimable` | `true` | `false` forbids new lands there (shipped `false` for `arena`, `spawn`, `world_nether`) |
| `forced` | `{}` | `key: ALLOW\|DENY` that overrides every land and the wilderness |
| `wilderness` | `{}` | Rules where there is no land; empty allows everything |
| `claimed-defaults` | `{}` | Answer inside a land when no member, role or visitor rule decides; for flags, the value when the land has none |

### Land flags

| Flag | On for new lands | Controls |
|---|---|---|
| `lands:pvp` | no | Players hurting each other |
| `lands:keep_inventory` | no | Keeping items on death |
| `lands:creeper_damage` | yes | Creeper explosions breaking blocks |
| `lands:wither_damage` | yes | The Wither breaking blocks |
| `lands:fire_spread` | yes | Fire spreading and burning blocks |
| `lands:lightning_damage` | no | Lightning fires and damage |
| `lands:mob_griefing` | no | Mobs changing blocks |
| `lands:fluid_flow` | yes | Water and lava flow |
| `lands:piston_push` | yes | Pistons moving blocks across the border |
| `lands:leaf_decay` | no | Leaf decay |
| `lands:ice_melt` | no | Ice and snow melting |
| `lands:block_fade` | no | Coral, farmland and similar fading |
| `lands:block_form` | yes | Ice, obsidian and similar forming |
| `lands:entity_block_form` | no | Snow golems and Frost Walker making blocks |
| `lands:structure_grow` | yes | Saplings and mushrooms growing |
| `lands:nature_spread` | yes | Grass, mycelium and vines spreading |
| `lands:spawn_animals` | yes | Natural animal spawns |
| `lands:spawn_monsters` | yes | Natural monster spawns |
| `lands:spawn_phantoms` | no | Phantom spawns |
| `lands:mob_spawning` | yes | Any mob spawning |
| `lands:enderman_pickup` | no | Endermen taking blocks |
| `lands:zombie_door_break` | no | Zombies breaking doors |
| `lands:silverfish_infest` | no | Silverfish hiding in stone |
| `lands:coral_fade` | no | Coral dying out of water |
| `lands:sculk_spread` | no | Sculk spreading |
| `lands:redstone` | yes | Redstone circuits |
| `lands:tnt_explosions` | yes | TNT breaking blocks |
| `lands:end_crystal_damage` | no | End crystal explosions |
| `lands:bed_explosion` | no | Beds exploding in the Nether and End |
| `lands:fireball_damage` | no | Ghast and blaze fireballs |
| `lands:dragon_damage` | no | The Ender Dragon breaking blocks |
| `lands:respawn_anchor_explosion` | no | Respawn anchors exploding |

## How a decision is made

For "may this player do this here", the first step with an answer wins: protection off in the world allows; a `forced` world value; admin bypass and the bypass nodes `uxmclaims.bypass.enter` and `uxmclaims.bypass.teleport`; outside a land, the `wilderness` rule; a land ban; then the land itself (area, member override, owner and role, visitor role); last the world's `claimed-defaults`, else deny. Flags follow the same idea: world off, `forced`, `wilderness`, an [area](areas.md) value, the land's value, `claimed-defaults`, the flag default.

## Placeholders

| Placeholder | Renders |
|---|---|
| `%uxmclaims_selected_name%` | Name of the land picked with `/claim select`, empty when none |
| `%uxmclaims_selected_id%` | Id of that land |
| `%uxmclaims_selected_has%` | `true` when a land is picked |

## Notes

- With the default `protection.admin-bypass: TOGGLE`, staff turn their bypass on with `/uxm bypass on`; it ends with `/uxm bypass off` or when they leave.
- With [Invites](invites.md) on, `/claim trust <player>` without a duration sends an invite instead of adding the member at once.
- A deleted land stays restorable for `retention.deleted-days`. Most modules keep its warps, bans and vault until then; bank, relations, taxes, towns and nations do not, so a restored land comes back without those.
- `/claim merge` deletes the absorbed land for good: its roles, flags, spawn, warps, bans and bank go with it, and its members join with the member role. There is no merge button in the menus.
- Temporary trust runs out on a 60 second timer; on a network only the leading server runs it.

Related: [Areas](areas.md), [Invites](invites.md), [Bans](bans.md), [Warps](warps.md), [Limits](limits.md), [Economy](economy.md)
