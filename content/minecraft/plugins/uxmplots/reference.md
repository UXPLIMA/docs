---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmPlots."
---

## Commands

Names and aliases are the defaults from `commands.conf`, which renames or disables each of them.

| Command | Aliases | Subcommands |
|---|---|---|
| `/plot` | `/plots` | `auto`, `claim`, `price`, `abandon`, `info`, `rate`, `rate part`, `feel`, `feel list`, `shop`, `buy`, `sell`, `sell cancel`, `rent`, `rent cancel`, `merge`, `template`, `template save`, `unmerge`, `renting`, `renting mine`, `slots`, `slots buy`, `market`, `owned`, `outline`, `sponsor`, `sponsor buy`, `follow`, `unfollow`, `mute`, `unmute`, `following`, `trust`, `untrust`, `ban`, `unban`, `name`, `clear`, `floor`, `floor list`, `wall`, `wall list`, `border`, `ground`, `biome`, `biome list`, `ground list`, `border list`, `reset`, `snapshot`, `snapshot list`, `snapshot restore`, `flag`, `tag`, `tags`, `visiting`, `hide`, `flags`, `home`, `tp`, `session open`, `session close`, `session join`, `session leave`, `session`, `sessions`, `members`, `report`, `chat`, `leaderboard`, `leaderboard show`, `level`, `tip`, `tips`, `sign`, `guestbook`, `browse`, `random`, `visit`, `lang`, `lang set`, `lang reset`, `lang set-player`, `lang reset-player` |
| `/contest` | `/contests` | `info`, `enter`, `join`, `invite`, `leave`, `vote`, `tour`, `scores`, `entries`, `hall`, `edit` |
| `/uxmplots` | `/plotadmin` | `reload`, `version`, `doctor`, `reports`, `report`, `inactive`, `staff`, `pregen stop`, `pregen start`, `world list`, `world`, `pin`, `unpin`, `reset`, `snapshot`, `owner`, `delete`, `unlock`, `redstone`, `phase`, `board plant`, `board uproot` |

## Permissions

| Permission | What it allows | Default |
|---|---|---|
| `uxmplots.language` | Choose the language you read this plugin in | true |
| `uxmplots.admin.language` | Set or clear the language another player reads | op |
| `uxmplots.claim` | Take the plot you are standing on | true |
| `uxmplots.abandon` | Give a plot of yours back | true |
| `uxmplots.info` | Read who owns the plot you are standing on | true |
| `uxmplots.home` | Go to a plot of your own | true |
| `uxmplots.visit` | Go to somebody else's plot | true |
| `uxmplots.trust` | Let somebody build on a plot of yours, and take it away again | true |
| `uxmplots.ban` | Keep somebody off a plot of yours, and let them back in | true |
| `uxmplots.name` | Write a name on a plot of yours | true |
| `uxmplots.flag` | Read and change the flags on a plot of yours | true |
| `uxmplots.biome` | Put one of the world's biomes on a plot of yours | true |
| `uxmplots.ground` | Fill what a plot of yours is made of under its floor | true |
| `uxmplots.wall` | Build the ring around a plot of yours out of a wall this world offers | true |
| `uxmplots.outline` | Draw the edge of a plot in the air for a few seconds | true |
| `uxmplots.clear` | Take down what you built on a plot of yours, keeping the ground | true |
| `uxmplots.floor` | Lay one of the world's floors on a plot of yours | true |
| `uxmplots.reset` | Put a plot of yours back as it was generated | true |
| `uxmplots.snapshot` | Take a picture of a plot of yours, and put one back | true |
| `uxmplots.rate` | Say what you think of somebody else's plot | true |
| `uxmplots.browse` | Open the gallery and go and look at what other people built | true |
| `uxmplots.guestbook` | Read a plot's guestbook, and leave a line on somebody else's | true |
| `uxmplots.contest` | Read what contests are running | true |
| `uxmplots.contest.vote` | Judge an entry in a contest, and walk the tour of what you have not judged | true |
| `uxmplots.contest.enter` | Enter a build into a contest, join a team on one, or withdraw | true |
| `uxmplots.contest.edit` | Build and change a contest in game, and remove one | op |
| `uxmplots.session` | Open a plot of yours for others to build on for a while, or join one | true |
| `uxmplots.report` | Say there is something wrong with somebody else's build | true |
| `uxmplots.chat` | Talk to the people standing in the plot you are in | true |
| `uxmplots.chat.spy` | Read every plot channel on the server | op |
| `uxmplots.contest.hall` | Read the hall of fame | true |
| `uxmplots.slots` | Read how much room you have, and buy more | true |
| `uxmplots.template` | Put a layout the operator keeps down on a plot of yours | true |
| `uxmplots.template.save` | Keep a plot as a layout the server hands out | op |
| `uxmplots.merge` | Hold plots of yours as one | true |
| `uxmplots.rent` | Put a plot of yours up to rent, or take one for a while | true |
| `uxmplots.sell` | Put a plot of yours up for sale and read the market | true |
| `uxmplots.shop` | Read what a plot may wear and buy one of it | true |
| `uxmplots.sponsor` | Read what a sponsored place in the gallery costs, and buy one | true |
| `uxmplots.feel` | Choose what a plot of yours sounds and looks like | true |
| `uxmplots.border` | Lay a border this world offers on the wall of a plot of yours | true |
| `uxmplots.follow` | Follow a builder and hear what they build | true |
| `uxmplots.leaderboard` | Read one of the boards | true |
| `uxmplots.level` | Read how far a plot has climbed and what raised it | true |
| `uxmplots.tip` | Tip the builder of a plot you are standing in | true |
| `uxmplots.tag` | Put a word on a plot of yours, so the gallery can find it | true |
| `uxmplots.visiting` | Say who may come to a plot of yours | true |
| `uxmplots.hide` | Keep a plot of yours out of the gallery | true |
| `uxmplots.bypass` | Build, break and open anywhere, whoever owns the plot | op |
| `uxmplots.gamemode.keep` | Keep the game mode you set yourself, in a world that forces one | false |
| `uxmplots.admin.*` | Everything an operator does with this plugin | op |
| `uxmplots.admin.reports` | Read the report queue | op |
| `uxmplots.admin.report` | Decide a report as valid, cleaned or unfounded | op |
| `uxmplots.admin.inactive` | Read every plot whose owner has stopped coming | op |
| `uxmplots.admin.doctor` | Ask how this plugin is | op |
| `uxmplots.admin.version` | Read the version this server runs | op |
| `uxmplots.admin.reload` | Read the configuration again | op |
| `uxmplots.admin.world` | Set a plot world up in game | op |
| `uxmplots.admin.phase` | End the phase a contest is in, so the next one starts now | op |
| `uxmplots.admin.reset` | Put any plot back as the generator made it | op |
| `uxmplots.admin.snapshot` | Take a picture of any plot | op |
| `uxmplots.admin.owner` | Hand any plot to somebody else | op |
| `uxmplots.admin.delete` | Take any plot back from its owner | op |
| `uxmplots.admin.unlock` | Let go of a contest lock on a plot, or put it back | op |
| `uxmplots.admin.redstone` | Switch a plot's redstone back on before its pause runs out | op |
| `uxmplots.admin.pregen` | Stop or start a world generating ahead of its players, until the server restarts | op |
| `uxmplots.admin.pin` | Put a build on the gallery's pinned row, or take it off | op |
| `uxmplots.admin.board` | Plant a board where you are standing, and take one down | op |
| `uxmplots.admin.lag` | Be told when a plot's redstone is switched off for running too fast | op |

## Placeholders

The plugin registers a PlaceholderAPI expansion when PlaceholderAPI is installed.

## Files

The plugin writes these into `plugins/uxmPlots/` on first start and reads them again on reload:

- `catalogues/personality.conf`
- `catalogues/shop.conf`
- `catalogues/tags.conf`
- `commands.conf`
- `config.conf`
- `contests.conf`
- `contests/example.conf`
- `currencies.conf`
- `discord.conf`
- `editor/contest.conf`
- `editor/world.conf`
- `hologram.conf`
- `levels.conf`
- `network.conf`
- `menus/bench.conf`
- `menus/board.conf`
- `menus/contest.conf`
- `menus/flags.conf`
- `menus/gallery.conf`
- `menus/guestbook.conf`
- `menus/hall.conf`
- `menus/inactive.conf`
- `menus/member.conf`
- `menus/members.conf`
- `menus/panel.conf`
- `menus/scorecard.conf`
- `menus/sessions.conf`
- `menus/shop.conf`
- `menus/staff.conf`
- `messages/messages_de.conf`
- `messages/messages_en.conf`
- `messages/messages_es.conf`
- `messages/messages_fr.conf`
- `messages/messages_pt.conf`
- `messages/messages_ru.conf`
- `messages/messages_tr.conf`
- `protection.conf`
- `rating.conf`
- `text-input.conf`
- `theme.conf`
- `worlds/creative.conf`
