---
title: Server operations
order: 7
description: Locking, lockdown, PVP, shutdowns, lighting, terrain, teams, leaderstats and warps.
icon: server
---

Seventy commands operate on the server rather than on a player. The dangerous ones are
Owner-only and confirmed in the panel before they run.

## Access control

| Command | Rank | Does |
|---|---|---|
| `lock` | Admin | New players cannot join |
| `unlock` | Admin | Lifts it |
| `lockdown` | HeadAdmin | Kicks everybody not on the whitelist, and keeps them out |
| `addlockdown` | HeadAdmin | Adds somebody to the whitelist |
| `removelockdown` | HeadAdmin | Removes somebody |
| `unlockdown` | HeadAdmin | Lifts it |

The difference: `lock` keeps new players out and leaves everybody already here alone.
`lockdown` clears the server of everybody not whitelisted.

Both are re-checked when a player joins, so a locked server keeps refusing joins until it is
unlocked.

<Callout type="tip" title="Lockdown is the tool for an incident">

An exploiter with friends is a stream of new accounts. `lockdown`, then whitelist your
staff, and the server is yours while you work out what happened.

`globallockdown` does it across every server at once, which is the version you want if the
problem is not confined to one.

</Callout>

## Ending a server

| Command | Rank | Does |
|---|---|---|
| `shutdown` | Owner | Warns, then closes this server |
| `closeserver` | Owner | Closes it |
| `restartserver` | Owner | Closes it and moves players into a fresh one |
| `gshutdown` | Owner | Shuts down every running server |
| `migrate` | HeadAdmin | Reloads this server onto the latest place version |
| `gmigrate` | Owner | Reloads every server onto the latest version |

`migrate` is the one to know: publishing an update does not move anybody, and existing
servers keep running the old code until they empty. Migrating moves your players onto the
new version now.

<Callout type="danger" title="The global ones reach every player in your game">

`gshutdown`, `globallockdown` and `gmigrate` are broadcast to every server. There is no
confirmation beyond the panel dialog and no undo.

They are Owner-only for a reason. Keep the Owner rank to people who understand what
"everybody, everywhere, right now" means.

</Callout>

## PVP

`pvpon` and `pvpoff`, at Admin. Off is the default: a joining player is given an invisible
force field named `PVPField` on every spawn, and turning PVP on removes them.

## Time and lighting

Twenty-odd commands write straight to `Lighting`.

| Group | Commands |
|---|---|
| Time | `time`, `sunrisetime`, `noontime`, `sunsettime`, `nighttime` |
| Basics | `brightness`, `ambient`, `outdoorambient`, `exposure`, `shadows` |
| Fog | `fog`, `fogstart`, `fogend`, `fogColor` |
| Environment | `diffusescale`, `specularscale`, `latitude`, `colorshift` |
| Atmosphere and wind | `atmosphere`, `winddir`, `windspeed` |
| Post effects | `grayscale`, `saturate`, `contrast`, `inverted`, `tint`, `unvisuals` |
| Skybox | `skybox`, which opens a picker over `Storage/Skybox` |

`unvisuals` clears every Lighting effect the system placed, which is the way back from a
session of experimenting.

<Callout type="warning" title="Lighting changes are not saved">

They live only in this server and are gone when it restarts. That makes them safe to play
with and useless as a permanent setting: change your game's actual lighting in Studio.

`atmosphere` creates an `Atmosphere` object if the game does not have one, which is a real
change to the place's lighting rig for as long as the server runs.

</Callout>

## Terrain and water

`clearterrain` and `colorterrain` at HeadAdmin and Admin, plus `watercolor`,
`waterreflectance`, `watertransparency`, `watersize` and `waterspeed`.

`clearterrain` deletes all terrain in the running server. It is confirmed in the panel and
it is not reversible except by restarting the server.

## The map

| Command | Rank | Does |
|---|---|---|
| `savemap` | Owner | Snapshots the workspace into storage |
| `loadmap` | Owner | Restores that snapshot |
| `restoremap` | Owner | Restores the original snapshot |
| `lockMap` | HeadAdmin | Anchors and locks every part in the workspace |

<Callout type="warning" title="Map saves live in this server only">

The snapshot goes into the storage folder of the running server. Restart and both the
snapshot and any changes you made are gone.

It is an undo button for a building session, not a save system.

</Callout>

## Teams

| Command | Rank | Does |
|---|---|---|
| `setteam` | HeadAdmin | Moves a player to a team |
| `createteam` | HeadAdmin | Creates one |
| `editteam` | HeadAdmin | Recolours one |
| `removeteam` | HeadAdmin | Deletes one |
| `clearteams` | HeadAdmin | Deletes them all |
| `teamrespawn` | HeadAdmin | Respawns everybody on a team |
| `randomizeteams` | HeadAdmin | Shuffles everybody across the existing teams |

<Callout type="danger" title="Teams can grant ranks">

If your `Permissions.Assignments` uses `Teams`, then `setteam` is a rank command wearing a
different hat: moving somebody onto the staff team gives them the staff rank.

Either keep team assignments out of the rank config, or accept that `setteam` needs the same
trust as `rank`.

</Callout>

## Leaderstats

| Command | Rank | Does |
|---|---|---|
| `setstat` | HeadAdmin | Sets a stat to a value |
| `addstat` | HeadAdmin | Adds to a numeric stat |
| `subtractstat` | HeadAdmin | Subtracts |
| `removestat` | HeadAdmin | Removes a stat |
| `resetstats` | HeadAdmin | Zeroes every stat |
| `editdata` | HeadAdmin | Opens a data editor covering nested values |

These write to the live `leaderstats` objects. Whether the change persists depends entirely
on your game's own saving: if your data system saves on leave, it saves whatever the admin
set.

## Warps

`setwarp` saves your current position under a name, `warp` teleports you to one, `delwarp`
removes one and `warps` lists them. HeadAdmin, except `warp` itself at Admin.

Warps live in the running server, so they are a per-session convenience rather than a
permanent map of your world.

## Inserting things

`insert` at HeadAdmin drops a marketplace asset into the workspace by id.

<Callout type="danger" title="Inserting an asset runs somebody else's code">

A model from the marketplace can contain scripts, and inserting it into a running server
runs them with full server permissions.

Anybody who can run `insert` can run arbitrary code in your game. Treat that rank as
equivalent to giving out Studio access, and prefer to insert nothing you have not opened in
Studio first.

</Callout>
