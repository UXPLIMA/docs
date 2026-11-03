---
title: Reference
order: 701
description: Every permission node of uxmPlots with its default and what it allows.
icon: list-checks
---

A node held by default (`true`) keeps its command visible to everybody; a player without it is refused with
`command.no-permission`. Any other node hides its command from a player who lacks it.

## Players

| Node | Default | Allows |
|---|---|---|
| `uxmplots.language` | true | Choose the language you read this plugin in |
| `uxmplots.claim` | true | Take the plot you stand on, `/plot auto`, `/plot price` |
| `uxmplots.abandon` | true | Give a plot of yours back |
| `uxmplots.info` | true | `/plot`, `/plot info`: who owns the plot you stand on |
| `uxmplots.home` | true | Go to a plot of your own |
| `uxmplots.visit` | true | Go to somebody else's plot |
| `uxmplots.trust` | true | Trust and untrust, the members window |
| `uxmplots.ban` | true | Ban and unban |
| `uxmplots.name` | true | Write a name on a plot of yours |
| `uxmplots.flag` | true | Read and change the flags on a plot of yours |
| `uxmplots.biome` | true | Put one of the world's biomes on a plot of yours |
| `uxmplots.ground` | true | Fill the ground under a plot's floor |
| `uxmplots.wall` | true | Build the ring out of a wall the world offers |
| `uxmplots.border` | true | Lay a border the world offers |
| `uxmplots.floor` | true | Lay one of the world's floors |
| `uxmplots.outline` | true | Draw the edge of a plot in the air |
| `uxmplots.clear` | true | Take down what you built, keeping the ground |
| `uxmplots.reset` | true | Put a plot of yours back as generated |
| `uxmplots.snapshot` | true | Take a picture of a plot of yours, and put one back |
| `uxmplots.rate` | true | Rate somebody else's plot |
| `uxmplots.browse` | true | The gallery and `/plot random` |
| `uxmplots.guestbook` | true | Read a guestbook, and sign somebody else's |
| `uxmplots.contest` | true | Read what contests are running |
| `uxmplots.contest.vote` | true | Judge an entry and walk the tour |
| `uxmplots.contest.enter` | true | Enter, join a team, invite, or withdraw |
| `uxmplots.contest.hall` | true | Read the hall of fame |
| `uxmplots.session` | true | Open a session, or join one |
| `uxmplots.report` | true | Report somebody else's build |
| `uxmplots.chat` | true | Talk to the people standing in your plot |
| `uxmplots.slots` | true | Read your room, and buy more |
| `uxmplots.template` | true | Put a layout the server keeps down on a plot of yours |
| `uxmplots.merge` | true | Merge and unmerge |
| `uxmplots.rent` | true | Rent a plot out, or take one |
| `uxmplots.sell` | true | Sell a plot, read the market and buy the plot for sale you stand on |
| `uxmplots.shop` | true | The shop, `/plot buy <kind> <entry>`, `/plot owned` |
| `uxmplots.sponsor` | true | Buy a sponsored place in the gallery |
| `uxmplots.feel` | true | Choose what a plot sounds and looks like |
| `uxmplots.follow` | true | Follow, unfollow, mute and unmute builders |
| `uxmplots.leaderboard` | true | Read the boards |
| `uxmplots.level` | true | Read a plot's level |
| `uxmplots.tip` | true | Tip a builder, and read a plot's tips |
| `uxmplots.tag` | true | Put a tag on a plot of yours |
| `uxmplots.visiting` | true | Say who may come to a plot of yours |
| `uxmplots.hide` | true | Keep a plot of yours out of the gallery |

## Staff

| Node | Default | Allows |
|---|---|---|
| `uxmplots.admin.language` | op | Set or clear another player's language |
| `uxmplots.contest.edit` | op | Build, change and remove a contest in game |
| `uxmplots.chat.spy` | op | Read every plot channel |
| `uxmplots.template.save` | op | Keep a plot as a layout |
| `uxmplots.bypass` | op | Build, break and open anywhere; forbidden blocks; not held by edits or crash rules |
| `uxmplots.admin.reports` | op | The report queue, the inactive window from it, and `/uxmplots staff` |
| `uxmplots.admin.report` | op | Decide a report |
| `uxmplots.admin.inactive` | op | Read every inactive plot, and release one |
| `uxmplots.admin.doctor` | op | `/uxmplots doctor` |
| `uxmplots.admin.version` | op | `/uxmplots version` |
| `uxmplots.admin.reload` | op | `/uxmplots reload` |
| `uxmplots.admin.world` | op | The world editor |
| `uxmplots.admin.phase` | op | End a contest's phase now |
| `uxmplots.admin.reset` | op | Reset any plot |
| `uxmplots.admin.snapshot` | op | Photograph any plot |
| `uxmplots.admin.owner` | op | Hand any plot to somebody else |
| `uxmplots.admin.delete` | op | Take any plot back, the blocks staying |
| `uxmplots.admin.unlock` | op | Lift or put back a contest lock |
| `uxmplots.admin.redstone` | op | Switch a plot's redstone back on early |
| `uxmplots.admin.pregen` | op | Stop or start pre-generation until a restart |
| `uxmplots.admin.pin` | op | Pin and unpin a build in the gallery |
| `uxmplots.admin.board` | op | Plant and uproot a board |
| `uxmplots.admin.lag` | op | Be told when a plot's redstone is paused |
| `uxmplots.gamemode.keep` | false | Keep your own game mode in a world that forces one |

`uxmplots.admin.*` (op) grants every `uxmplots.admin.` node above.

## Nodes you name

Each is held by operators alone until you give it. The three shipped examples (`floor.gold-veined`, `biome.snowy`,
`music.pigstep`) are declared that way so a permissions editor lists them; the others are undeclared, which Bukkit
treats the same.

| Node | Gives |
|---|---|
| `uxmplots.limit.world.<n>` | `n` plots in each world; the highest wins; `unlimited` for no limit |
| `uxmplots.limit.total.<n>` | `n` plots across all worlds; the highest wins; `unlimited` |
| `uxmplots.floor.gold-veined` | The shipped `gold-veined` floor without buying it |
| `uxmplots.biome.snowy` | The shipped `snowy` biome |
| `uxmplots.music.pigstep` | The shipped `pigstep` music |
| Any `permission` in a world, catalogue or tag file | That entry |
| `jury-permission` in a contest file | A seat on that contest's jury |

`fawe.uxmplots` is FastAsyncWorldEdit's own node for editing inside a plot; everybody holds it unless you take it away.

Related: [Commands](../commands/overview.md)
