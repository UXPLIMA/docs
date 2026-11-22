---
title: Rollback
order: 1333
description: Snapshots of a land's chunks, put back by staff, by owners, or when a land ends.
icon: history
---

Rollback stores a land's chunks as snapshots: blocks, container contents, sign texts, armour stands and item
frames. Staff take and restore them with `/uxm rollback`, owners see and restore their own with `/claim
history`, and the ground of a land that ends can be put back on its own. With CoreProtect installed, staff can
also roll a land back through CoreProtect.

Module `rollback` · disabled by default · `modules/rollback/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm rollback snapshot <land> [reason]` | Stores every chunk of a land. | `uxmclaims.admin.rollback` |
| `/uxm rollback list <land>` | Lists a land's snapshots, newest first. | `uxmclaims.admin.rollback` |
| `/uxm rollback restore <land> [number] [physics]` | Puts a snapshot back; `1` is the newest and the default. `physics` runs block physics for this restore. | `uxmclaims.admin.rollback` |
| `/uxm rollback cancel <land>` | Stops a land's running restore. | `uxmclaims.admin.rollback` |
| `/uxm rollback coreprotect <land> <time>` | Rolls the land's chunks back through CoreProtect, for example `12h` or `3d`. | `uxmclaims.admin.rollback` |
| `/claim history` | Lists the snapshots of the land you stand in. | `uxmclaims.command.rollback.history` |
| `/claim history restore <number>` | Puts a snapshot of your land back. | `uxmclaims.command.rollback.history` + `uxmclaims.command.rollback.history.restore` |
| `/claim history cancel` | Stops the restore running in the land you stand in. | `uxmclaims.command.rollback.history` |

Land names may contain spaces. Times take `s`, `m`, `h`, `d` and `w`, alone or summed (`1d12h`). A restore reports
its progress to whoever started it.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.rollback` | op | The `/uxm rollback` commands; `/claim history` in any land, free restores with no `owner-restore` check. |
| `uxmclaims.command.rollback.history` | everyone | `/claim history` in a land you own. |
| `uxmclaims.command.rollback.history.restore` | everyone | `/claim history restore`, when `history.owner-restore` is on. |

| Cost key | Default | What it pays for |
|---|---|---|
| `rollback:restore` | `0` | One `/claim history restore` by an owner. Set it in `modules/economy/costs.yml`. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `snapshot-on-create` | `false` | Takes a snapshot of a land when it is created. |
| `snapshot-on-chunk-claim` | `false` | Takes a snapshot of each chunk claimed into an existing land. |
| `max-snapshots-per-land` | `5` | Snapshots a land keeps; one more deletes the oldest. At least 1. |
| `keep-claim-snapshots` | `true` | Keeps the snapshots taken at claim time out of `max-snapshots-per-land`, so `on-unclaim: snapshot` can always put the land back as it was before. They go when the land is purged. |
| `max-chunks` | `256` | Lands with more chunks are not snapshotted. |
| `block-entities` | `true` | Stores container items, sign text, banner patterns, skull owners, spawners, lecterns, jukeboxes and campfires. |
| `entities` | `true` | Stores armour stands (not markers) and item frames. A restore removes the chunk's armour stands and item frames and puts the stored ones back. |
| `blocks-per-tick` | `16384` | Blocks a restore sets per chunk per tick. Lower it if restoring makes the server lag. |
| `restore-physics` | `false` | Runs block physics while restoring. Off sets blocks exactly as stored. |
| `progress-every-percent` | `25` | How often a restore reports progress; `0` only at the end. |
| `coreprotect-max-time` | `30d` | The longest time `/uxm rollback coreprotect` and the `coreprotect` strategy may go back. |
| `on-unclaim.unclaim` | `none` | Strategy for a chunk unclaimed from a land. |
| `on-unclaim.delete` | `none` | Strategy for a land deleted by its owner or an admin. |
| `on-unclaim.expiry` | `none` | Strategy for a land whose time ran out ([Expiry](expiry.md)). |
| `on-unclaim.ruin` | `none` | Strategy for a ruined land, or a land of a ruined town, deleted at the end of its ruin ([Ruins](ruins.md)). |
| `on-unclaim.plugin` | `none` | Strategy for a land deleted by another plugin or module. |
| `history.owner-restore` | `true` | Owners may put a snapshot of their land back. Only the chunks still in the land are restored. |

## When a land ends

Each `on-unclaim` cause takes one strategy:

| Strategy | What happens to the chunks |
|---|---|
| `none` | Nothing; the blocks stay as they are. |
| `snapshot` | Put back from the land's oldest snapshot that holds each chunk. Needs `snapshot-on-create` or `snapshot-on-chunk-claim`. |
| `coreprotect` | CoreProtect rolls them back to when the land was created, at most `coreprotect-max-time`. On Folia this falls back to `snapshot`. |
| `regenerate` | The server generates them anew. Where the server does not support this, `snapshot` is used. |

Chunks claimed again by then are never touched. With a snapshot strategy and both claim-time options off, the
module logs a warning at start: only snapshots taken by command or before a war are there to use.

<Callout type="warning" title="Turn on claim-time snapshots first">

`on-unclaim: snapshot` can only put back ground it has a snapshot of. Turn on `snapshot-on-create` and
`snapshot-on-chunk-claim` before lands are claimed; lands claimed earlier have nothing to restore from.

</Callout>

## Notes

- With the [War](war.md) module running, every land of the defending side is snapshotted (reason `war`) before a war starts.
- `/claim history` is for the land's owner and staff only; members cannot use it, whatever their role.
- The owner's restore price is taken when the snapshot is read and is not refunded if the restore fails or is cancelled.
- `/uxm rollback coreprotect` does not run on Folia. CoreProtect is reached without a build dependency, and a border chunk takes four overlapping CoreProtect calls. The CoreProtect lookup form of `/claim history` (listing CoreProtect records) is not there yet.
- Not stored: other plugins' data on block entities, beehive bees, furnace and brewing timers, paintings, mobs and dropped items. A chunk whose entities are not loaded is stored or restored without them.
- A land brought back with `/uxm land restore` gets its blocks back only from a snapshot, because the strategy ran when it was deleted. A land merged into another loses its snapshots with the purge.

Related: [Lands](lands.md), [Expiry](expiry.md), [Ruins](ruins.md), [War](war.md), [Integrations](../integrations.md)
