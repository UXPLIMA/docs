---
title: Commands
order: 1060
description: Every /guard subcommand, with its arguments, what it does and the node it needs.
icon: terminal
---

Contents: [Root and words](#root-and-words) · [For everyone](#for-everyone) · [Drawing](#drawing) ·
[Defining](#defining) · [Rules, roles and behaviours](#rules-roles-and-behaviours) ·
[Organising](#organising) · [Looking around](#looking-around) · [History and logs](#history-and-logs) ·
[Running the module](#running-the-module)

## Root and words

The command is `/guard`, and `/bolge` in Turkish. Set `commands.rg-alias = true` in `modules/guard/config.conf`
to also answer `/rg`; it is off by default so WorldGuard keeps `/rg` while both are installed. Like every
uxmEssentials command it can be renamed, re-aliased or switched off in `commands.conf` under the id `guard`.

`/guard` alone opens the [menu](menus.md) for staff with the `menu` node and for a region owner or manager
standing in their region; anyone else gets the help list. `/guard help` lists the subcommands you have the node
for. Every
node below starts with `uxmessentials.guard.`; [Permissions](../modules/guard.md#permissions) has the defaults.

`<region>` is a region name in the world you stand in, or `world:name` for another world. A command typed
from the console needs the `world:` form.

## For everyone

These nodes default to every player.

| Command | What it does | Node |
|---|---|---|
| `/guard menu` | Opens the region list (with the `menu` node), or, for a region owner or manager without it, the roles window of the region they stand in | `use` |
| `/guard help` | Lists the subcommands you may run | `use` |
| `/guard actions [prefix]` | Lists the action tree: the top level, or every action under a prefix such as `use` | `use` |
| `/guard here` | The regions where you stand, highest first, and your role in each | `here` |
| `/guard why <action>` | Explains whether you may do the action where you stand: each region asked, the rule that decided, and where it came from | `why` |
| `/guard member <region> <player> <member\|guest\|clear>` | An owner or manager hands out or takes back `member` and `guest` in their own region. Staff with `role` may do it anywhere | `member` |

## Drawing

| Command | What it does | Node |
|---|---|---|
| `/guard wand` | Gives the guard wand, a blaze rod. Left click: first corner. Right click: second corner. Sneak and right click: a polygon point | `define` |
| `/guard pos1`, `/guard pos2` | Sets the first or second corner where you stand | `define` |
| `/guard point` | Adds a polygon point where you stand | `define` |
| `/guard clear` | Forgets your selection | `define` |
| `/guard expand <amount> <direction>` | Grows the selection by blocks: `up`, `down`, `north`, `south`, `east`, `west` or `all` | `define` |
| `/guard expand vert` | Stretches the selection to the full height of the world | `define` |

With WorldEdit or FAWE installed, a selection made with their wand is used as well.

## Defining

| Command | What it does | Node |
|---|---|---|
| `/guard define <name> [shape]` | Makes a region. Shapes: `cuboid` (the default, from your selection), `polygon [minY] [maxY]`, `cylinder <radius> [height]`, `sphere <radius>`, `chunks [radius]`, `global`. See [Shapes](concepts.md#shapes) | `define` |
| `/guard redefine <region> [shape]` | Gives a region a new shape and keeps everything else | `define` |
| `/guard remove <region>` | Removes a region. The menu asks first; the command does not | `remove` |
| `/guard restore <region>` | Brings back a removed region as it was | `remove` |

## Rules, roles and behaviours

| Command | What it does | Node |
|---|---|---|
| `/guard rule <region> <action> <target> <allow\|deny\|clear>` | Sets or clears one rule. Targets: `everyone`, `nonmember`, `role:<name>`, `perm:<node>`, `cond:<condition>`; a bare role name also works. See [Rules](rules.md) | `rule` |
| `/guard role <region> <player> <role\|clear>` | Gives any role, `owner` and `manager` included, or takes it away | `role` |
| `/guard set <region> <behaviour> <value\|clear>` | Sets or clears a behaviour. A value that does not fit the behaviour is refused with the reason. See [Behaviours](behaviours.md) | `set` |
| `/guard template <region> <template\|clear>` | Links the region to a template, or unlinks it. See [Templates](templates.md) | `template` |

## Organising

| Command | What it does | Node |
|---|---|---|
| `/guard priority <region> <number>` | Sets the priority. Higher wins where regions overlap | `priority` |
| `/guard parent <region> <parent\|clear>` | Makes the region inherit from another one in the same world | `parent` |
| `/guard family <region> <name\|clear>` | Puts the region in a family, so blocks may cross between it and its relatives. See [Parents and families](concepts.md#parents-and-families) | `family` |
| `/guard tag <region> add <tag>`, `/guard tag <region> remove <tag>` | Adds or removes a tag | `tag` |
| `/guard tag bulk <tag> rule <action> <target> <allow\|deny\|clear>` | Sets one rule on every region with the tag | `tag` |
| `/guard tag bulk <tag> set <behaviour> <value\|clear>` | Sets one behaviour on every region with the tag | `tag` |
| `/guard tag bulk <tag> template <template\|clear>` | Links every region with the tag to a template | `tag` |
| `/guard scope <region> <server\|set:name>` | Chooses which servers load the region. See [Networks](networks.md) | `scope` |
| `/guard lock <region>`, `/guard unlock <region>` | Locks or unlocks a region. See [Locked regions](concepts.md#locked-regions-and-locked-worlds) | `lock` |

A bulk edit writes one history entry per region, so each region can be undone on its own.

## Looking around

| Command | What it does | Node |
|---|---|---|
| `/guard info [region]` | Shape, scope, priority, parent, family, template, tags, lock, rules, behaviours and roles. Without a name, the top region where you stand | `info` |
| `/guard list [world] [page]` | The regions of a world, highest priority first | `list` |
| `/guard near [radius]` | Regions within the radius around you, 32 blocks by default and at most 512 | `near` |
| `/guard show [region]` | Draws borders for you alone: the named region, or every region near you plus your selection | `show` |
| `/guard hide` | Takes the borders away again | `show` |
| `/guard test <player> <action> [region]` | Whether that player may do the action where you stand, or inside the named region, and why | `test` |
| `/guard inspect` | Inspect mode on or off. A click on a block explains what a player with no role may do there; your own refusals are explained as they happen | `inspect` |

## History and logs

| Command | What it does | Node |
|---|---|---|
| `/guard history [region] [page]` | Recent changes, newest first: of one region, or of all of them. Each line has an id | `history` |
| `/guard undo <id>` | Undoes one change by its history id. The undo is recorded too, so it can be undone | `undo` |
| `/guard log [player\|region:<name>] [page]` | Refused actions, when the denial log is on (`log.enabled`) | `log` |

## Running the module

| Command | What it does | Node |
|---|---|---|
| `/guard bypass` | Rules stop applying to you until you run it again, except inside a locked region without the `lock` node. A reminder shows every `bypass.reminder-seconds` | `bypass` |
| `/guard status` | Regions, chunk columns and region sets in memory, checks per second, and the cost per action family | `status` |
| `/guard import worldguard <dry-run\|run\|undo> [yaml\|sql]` | Imports WorldGuard regions. See [Importing WorldGuard](worldguard.md) | `import` |
| `/guard backups` | Lists the region backups, newest first. See [Backups](tools.md#backups) | `backups` |
| `/guard backups restore <file>` | Puts every region of a backup back, after saving the present state as a new backup | `backups` |
| `/guard reload` | Reads `modules/guard/config.conf` and the window files again, and reloads every region from the database | `reload` |
