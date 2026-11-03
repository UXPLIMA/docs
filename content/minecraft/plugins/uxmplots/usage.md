---
title: "Guide"
order: 10
description: "How uxmPlots works, how to set it up and what each file holds."
---

uxmPlots runs plot worlds for creative and building servers: a grid of plots and roads, claiming and trust, a gallery with ratings and visits, guestbooks and tips, shared building sessions, contests with judging and a hall of fame, plot levels, a cosmetic shop, renting and selling, snapshots, and protection against griefing and crash items.

## Install

1. Put the jar in `plugins/`.
2. Start the server once. The plugin writes its files into `plugins/uxmPlots/` and, with `create-world = true` in `worlds/creative.conf`, creates the world `creative` with its own generator.
3. Adjust the world file and `config.conf`, then run `/uxmplots reload`.

On Folia a world cannot be created while the server runs: add it to the worlds section of `bukkit.yml` with `generator: uxmPlots`, as the log says. WorldEdit or FastAsyncWorldEdit, an economy, PlaceholderAPI and Discord are optional.

## Files

| File | What it holds |
|---|---|
| `config.conf` | everything that is not one world, one contest or one catalogue (sections below) |
| `protection.conf`, `rating.conf`, `contests.conf`, `network.conf` | the settings of the four largest features, each under its own block name; a block an earlier release kept in `config.conf` moves here on the first start |
| `worlds/<world>.conf` | one plot world; the file name is the world name |
| `contests/<id>.conf` | one contest; the file name is the id |
| `catalogues/shop.conf`, `personality.conf`, `tags.conf` | what the shop sells, the sounds and looks a plot may wear, and the tags |
| `currencies.conf` | the currencies prices, fees, rents and tips are paid in |
| `levels.conf` | what raises a plot's level, and what a level is worth |
| `discord.conf` | webhook lines for plot and contest events |
| `hologram.conf`, `text-input.conf` | the plot hologram and typed input |
| `editor/`, `menus/*.conf`, `commands.conf`, `messages/`, `theme.conf` | in-game editors, windows, commands, words, colours |

Every shipped file is commented in full; this page is the map.

## A plot world

| Key | Meaning |
|---|---|
| `create-world` | create the world on start when the server does not have it |
| `plot-size`, `road-size` | plot width and road width in blocks; a road of `0` puts plots against each other |
| `origin.x`, `origin.z` | shift the whole grid, for example to keep spawn on a road |
| `bottom-y`, `layers` | the ground from the bottom up, as `"<material> <depth>"`; the last layer is the plot floor |
| `walls` | `body`, `unclaimed-top` and `claimed-top`; the top tells passers-by which plots are free |
| `road-material`, `biome` | the road surface and the biome painted over the world |
| `max-plots-per-player` | this world's own limit; `0` leaves only the total limit |
| `visit-policy` | what a new plot starts as: `open`, `trusted` or `closed`; owners change it with `/plot visiting` |

The grid, layers, walls, road and biome are written by the generator. Changing them after the world exists is refused, because existing plots would no longer match. Plots are numbered from the origin: with plots of 64 and roads of 7, plot `0;0` covers blocks 0 to 63 and plot `1;0` starts at 71. A plot id is written `world:x;z`, such as `creative:-3;5`.

## Claiming and limits

`/plot claim` takes the plot you stand on, `/plot auto` finds the nearest free plot within `plots.auto-search-rings` rings (in `plots.auto-world` when you are outside every plot world). `/plot price` shows what your next plot costs.

A player's limits come from nodes: `uxmplots.limit.world.<n>` per world and `uxmplots.limit.total.<n>` across all worlds, the highest number held winning, or `unlimited`. Without a node the world's `max-plots-per-player` and `limits.default-total` apply. `/plot slots-buy` sells room for one more plot, when `slots` is set up.

## Merging

`/plot merge <side>` joins your plot with your neighbour on that side, rebuilding the road between them and, when four plots meet, the crossing. `/plot unmerge` splits them again. The `merge` section holds its rules.

## Config sections

| Section | What it decides |
|---|---|
| `plots` | name length and the auto claim search |
| `limits` | the limit node prefixes and the default total |
| `storage` | SQLite by default, or a shared `mysql`, `mariadb` or `postgresql` database |
| `protection` | what an untrusted player may do, and what they are told |
| `work` | how fast block rewrites (clear, reset, merge) run |
| `snapshots` | pictures of a plot that can be put back |
| `rating`, `visits` | what makes a rating and a visit count |
| `guestbook`, `tips` | notes visitors leave, and tipping the builder |
| `sessions` | owners opening a plot for others to build on for a while |
| `expiry` | what happens to plots whose owners stopped coming |
| `reports` | reporting a build and the staff queue |
| `leaderboards`, `follow` | boards on a wall, and following builders |
| `featured` | the pinned row staff fill and the sponsored row players buy, above the gallery ranking; neither moves the ranking |
| `contests`, `archive` | contest defaults, and the kept winning builds |
| `personality` | what a plot sounds and looks like to stand in |
| `merge` | how many plots may be held as one; `1` turns merging off |
| `crash-items` | items a client or server cannot survive, refused on a plot; every rule is off by default |
| `slots` | buying room for another plot |
| `network` | how servers on one database hear from each other |

## Contests

A contest file sets its phases and times, worked out from the clock so a restart loses nothing. Players enter with `/contest enter <contest> [team]`, invite teammates, then judge with `/contest tour` and `/contest vote`. Staff end a phase early with `/uxmplots phase <contest>`. Winners go to the archive and `/contest hall`.

## Commands

| Command | Permission | What it does |
|---|---|---|
| `/contest edit <contest>` | `uxmplots.contest.edit` | build and change a contest in game |
| `/contest enter <contest> [team]` | `uxmplots.contest.enter` | read what contests are running |
| `/contest entries <contest>` | `uxmplots.contest` | read what is entered in a contest |
| `/contest hall` | `uxmplots.contest.hall` | read the hall of fame |
| `/contest info <contest>` | `uxmplots.contest` | read one contest |
| `/contest invite <contest> <player>` | `uxmplots.contest.enter` | ask somebody onto your contest team |
| `/contest join <contest>` | `uxmplots.contest.enter` | join a team on a contest entry |
| `/contest leave <contest>` | `uxmplots.contest.enter` | take your build out of a contest |
| `/contest scores <contest>` | `uxmplots.contest` | read what a contest has scored so far |
| `/contest tour [contest]` | `uxmplots.contest.vote` | walk the entries you have not judged |
| `/contest vote [contest] [stars]` | `uxmplots.contest.vote` | walk the entries, or score the one you stand in |
| `/plot abandon` | `uxmplots.abandon` | give a plot of yours back |
| `/plot auto` | `uxmplots.claim` | take the next free plot and go to it |
| `/plot ban <player>` | `uxmplots.ban` | keep somebody off a plot of yours |
| `/plot biome <biome>` | `uxmplots.biome` | put one of the world's biomes on a plot of yours |
| `/plot biome-list` | `uxmplots.biome` | read the biomes this world offers |
| `/plot border-list` | `uxmplots.border` | read the borders this world offers |
| `/plot border <pattern>` | `uxmplots.border` | lay a border on the wall of a plot of yours |
| `/plot browse` | `uxmplots.browse` | open the gallery |
| `/plot buy [kind] [entry]` | `uxmplots.shop` | buy something a plot may wear |
| `/plot chat` | `uxmplots.chat` | talk to the people standing in this plot |
| `/plot claim` | `uxmplots.claim` | take the plot you are standing on |
| `/plot clear` | `uxmplots.clear` | take down what you built, keeping the ground |
| `/plot feel <kind> [choice]` | `uxmplots.feel` | choose what a plot of yours sounds and looks like |
| `/plot feel-list <kind>` | `uxmplots.feel` | read what one kind has on offer |
| `/plot flag <flag> [value]` | `uxmplots.flag` | read and change the flags on a plot of yours |
| `/plot flags` | `uxmplots.flag` | read every flag and what it does |
| `/plot floor-list` | `uxmplots.floor` | read the floors this world offers |
| `/plot floor <pattern>` | `uxmplots.floor` | lay one of the world's floors on a plot of yours |
| `/plot following` | `uxmplots.follow` | read who you follow |
| `/plot follow <player>` | `uxmplots.follow` | follow a builder and hear what they build |
| `/plot ground-list` | `uxmplots.ground` | read the grounds this world offers |
| `/plot ground <pattern>` | `uxmplots.ground` | fill what a plot of yours is made of under its floor |
| `/plot guestbook` | `uxmplots.guestbook` | read a plot's guestbook |
| `/plot hide [value]` | `uxmplots.hide` | keep a plot of yours out of the gallery |
| `/plot home [number]` | `uxmplots.home` | go to a plot of your own |
| `/plot info` | `uxmplots.info` | read who owns the plot you are standing on |
| `/plot leaderboard-show <board>` | `uxmplots.leaderboard` | read one board in full |
| `/plot leaderboard` | `uxmplots.leaderboard` | read one of the boards |
| `/plot level` | `uxmplots.level` | read how far a plot has climbed |
| `/plot market` | `uxmplots.sell` | read what is for sale |
| `/plot members` | `uxmplots.trust` | see who builds here and who is kept off |
| `/plot merge <side>` | `uxmplots.merge` | hold plots of yours as one |
| `/plot mute <player>` | `uxmplots.follow` | keep following somebody without hearing about it |
| `/plot name [name]` | `uxmplots.name` | write a name on a plot of yours |
| `/plot outline` | `uxmplots.outline` | draw the edge of a plot in the air for a few seconds |
| `/plot owned` | `uxmplots.shop` | read what you have bought from the shop |
| `/plot price` | `uxmplots.claim` | read what your next plot costs |
| `/plot random` | `uxmplots.visit` | go to a plot nobody sent you to |
| `/plot rate-part <part> <stars>` | `uxmplots.rate` | rate one part of a build |
| `/plot rate <stars>` | `uxmplots.rate` | say what you think of somebody else's plot |
| `/plot rent-cancel` | `uxmplots.rent` | stop offering a plot of yours to rent |
| `/plot renting-mine` | `uxmplots.rent` | read what you rent and rent out |
| `/plot renting` | `uxmplots.rent` | read what you are renting and what you rent out |
| `/plot rent [price] [currency] [days]` | `uxmplots.rent` | put a plot of yours up to rent, or take one |
| `/plot report <reason>` | `uxmplots.report` | say there is something wrong with a build |
| `/plot reset` | `uxmplots.reset` | put a plot of yours back as it was generated |
| `/plot sell-cancel` | `uxmplots.sell` | take a plot of yours off the market |
| `/plot sell <price> [currency]` | `uxmplots.sell` | put a plot of yours up for sale |
| `/plot session-close` | `uxmplots.session` | close a building session |
| `/plot session-join` | `uxmplots.session` | join somebody's building session |
| `/plot session-leave` | `uxmplots.session` | leave a building session |
| `/plot session-open <minutes>` | `uxmplots.session` | open a plot of yours for others to build on |
| `/plot sessions` | `uxmplots.session` | see the sessions you can join |
| `/plot session` | `uxmplots.session` | read the sessions that are open |
| `/plot shop [kind]` | `uxmplots.shop` | read what a plot may wear |
| `/plot sign <note>` | `uxmplots.guestbook` | leave a line in somebody's guestbook |
| `/plot slots-buy` | `uxmplots.slots` | buy room for another plot |
| `/plot slots` | `uxmplots.slots` | read how much room you have, and buy more |
| `/plot snapshot-list` | `uxmplots.snapshot` | read the pictures you have taken |
| `/plot snapshot-restore <name>` | `uxmplots.snapshot` | put one of your pictures back |
| `/plot snapshot` | `uxmplots.snapshot` | take a picture of a plot of yours |
| `/plot sponsor-buy` | `uxmplots.sponsor` | buy a sponsored place in the gallery |
| `/plot sponsor` | `uxmplots.sponsor` | read what a sponsored place in the gallery costs |
| `/plot tags` | `uxmplots.tag` | read the words a plot wears |
| `/plot tag <tag>` | `uxmplots.tag` | put a word on a plot of yours |
| `/plot template [name]` | `uxmplots.template` | put a layout the operator keeps down on a plot of yours |
| `/plot template-save [as]` | `uxmplots.template.save` | keep a plot as a layout |
| `/plot tip <amount> [currency]` | `uxmplots.tip` | tip the builder of a plot you are standing in |
| `/plot tips` | `uxmplots.tip` | read what a plot has been tipped |
| `/plot tp <plot>` | `uxmplots.visit` | go to a plot by its coordinates |
| `/plot trust <player> [grades]` | `uxmplots.trust` | let somebody build on a plot of yours |
| `/plot unban <player>` | `uxmplots.ban` | let somebody back in |
| `/plot unfollow <player>` | `uxmplots.follow` | stop following a builder |
| `/plot unmerge` | `uxmplots.merge` | split a plot of yours back up |
| `/plot unmute <player>` | `uxmplots.follow` | hear from somebody you follow again |
| `/plot untrust <player>` | `uxmplots.trust` | take that away again |
| `/plot visiting <policy>` | `uxmplots.visiting` | say who may come to a plot of yours |
| `/plot visit <target>` | `uxmplots.visit` | go to somebody's plot, or to a plot |
| `/plot wall-list` | `uxmplots.wall` | read the walls this world offers |
| `/plot wall <pattern>` | `uxmplots.wall` | build the ring around a plot of yours |
| `/uxmplots board-plant <board> <name>` | `uxmplots.admin.board` | plant a board where you are standing |
| `/uxmplots board-uproot <name>` | `uxmplots.admin.board` | take a board down |
| `/uxmplots delete` | `uxmplots.admin.delete` | take any plot back from its owner |
| `/uxmplots doctor` | `uxmplots.admin.doctor` | ask how this plugin is |
| `/uxmplots inactive` | `uxmplots.admin.inactive` | read every plot whose owner has stopped coming |
| `/uxmplots owner <player>` | `uxmplots.admin.owner` | hand any plot to somebody else |
| `/uxmplots phase <contest>` | `uxmplots.admin.phase` | end the phase a contest is in |
| `/uxmplots pin` | `uxmplots.admin.pin` | put a build on the gallery's pinned row |
| `/uxmplots pregen-start <world>` | `uxmplots.admin.pregen` | start a world generating ahead again |
| `/uxmplots pregen-stop <world>` | `uxmplots.admin.pregen` | stop a world generating ahead |
| `/uxmplots redstone` | `uxmplots.admin.redstone` | switch a plot's redstone back on before its pause runs out |
| `/uxmplots reload` | `uxmplots.admin.reload` | read the configuration again |
| `/uxmplots report <decision> <plot>` | `uxmplots.admin.report` | decide a report |
| `/uxmplots reports` | `uxmplots.admin.reports` | read the report queue |
| `/uxmplots reset` | `uxmplots.admin.reset` | put any plot back as the generator made it |
| `/uxmplots snapshot` | `uxmplots.admin.snapshot` | take a picture of any plot |
| `/uxmplots staff` | `uxmplots.admin.reports` | open the staff bench over the plot you stand on |
| `/uxmplots unlock` | `uxmplots.admin.unlock` | let go of a contest lock on a plot, or put it back |
| `/uxmplots unpin` | `uxmplots.admin.pin` | take a build off the pinned row |
| `/uxmplots version` | `uxmplots.admin.version` | read the version this server runs |
| `/uxmplots world-list` | `uxmplots.admin.world` | read every plot world this server runs |
| `/uxmplots world <world>` | `uxmplots.admin.world` | set a plot world up in game |

`/plot visit <target>` takes a player name or a plot id. `/plot trust <player> [grades]` takes optional grades of trust. The names and aliases come from `commands.conf`. The [reference](reference.md) lists the default aliases and every permission.

## Placeholders

| Placeholder | Value |
|---|---|
| `%uxmplots_plot_id%` | where the plot is, such as creative:-3;5 |
| `%uxmplots_plot_world%` | the world it is in |
| `%uxmplots_plot_owner%` | who owns it, or empty on a free plot |
| `%uxmplots_plot_name%` | what the owner called it, or empty |
| `%uxmplots_plot_members%` | how many players are trusted on it |
| `%uxmplots_plot_banned%` | how many are banned from it |
| `%uxmplots_plot_rating%` | the average out of five, or empty when unrated |
| `%uxmplots_plot_votes%` | how many people have rated it |
| `%uxmplots_plot_visits%` | how many different people have been there |
| `%uxmplots_plot_tags%` | the words it wears, separated by commas |
| `%uxmplots_plot_age_days%` | how many days ago it was claimed |
| `%uxmplots_plot_level%` | how far the plot has climbed |
| `%uxmplots_plot_level_points%` | what it has earned altogether |
| `%uxmplots_plot_level_next%` | points to the next level, or empty at the top |
| `%uxmplots_plot_tips%` | what it has been tipped, in every money anybody has tipped it in, or empty when nobody has |
| `%uxmplots_plot_honours%` | how many contests it has taken an honoured place in, or empty when none |
| `%uxmplots_plot_honours_best%` | the best place it has taken, or empty when none |
| `%uxmplots_plot_note_author%` | who wrote the guestbook note its owner pinned, or empty when none is pinned |
| `%uxmplots_plot_note%` | the guestbook note its owner pinned, or empty when none is pinned |
| `%uxmplots_contest_name%` | the running contest, which is the one building, locked or being judged, and of several the first by id; empty between contests |
| `%uxmplots_contest_id%` | its id, as its file is called |
| `%uxmplots_contest_phase%` | the phase it is in, as its id |
| `%uxmplots_contest_phase_word%` | the same, in the reader's language |
| `%uxmplots_contest_left%` | whole minutes until that phase ends |
| `%uxmplots_contest_time_left%` | the same, written out in the reader's language |
| `%uxmplots_contest_entered%` | true when this player has entered it |
| `%uxmplots_contest_entries%` | how many entries it holds |
| `%uxmplots_contest_<id>_<field>%` | any of those eight, for the contest of that id rather than the running one |
| `%uxmplots_owned%` | how many plots this player holds anywhere |
| `%uxmplots_owned_here%` | how many they hold in the world they are standing in |
| `%uxmplots_limit%` | how many they may hold anywhere, or empty when uncapped |
| `%uxmplots_in_plot%` | `true` or `false` |
| `%uxmplots_is_owner%` | whether they own the plot they are standing in |
| `%uxmplots_board_<board>_<place>_name%` | what that line is called |
| `%uxmplots_board_<board>_<place>_value%` | what it scored |
| `%uxmplots_board_<board>_<place>_plot%` | where the plot is |

The `plot_` family answers about the plot the reader stands in.

## Troubleshooting

- **A world will not start.** On Folia, add it to `bukkit.yml` with `generator: uxmPlots`.
- **A change to a world file is refused.** The world exists and its grid or layers would no longer match.
- **A WorldEdit edit leaves blocks unchanged.** Blocks outside the plot the player may build on are not written; `uxmplots.bypass` lifts this for staff.
- **`/plot auto` finds nothing.** Raise `plots.auto-search-rings`, or the world is full.
