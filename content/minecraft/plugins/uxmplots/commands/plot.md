---
title: Plot commands
order: 302
description: Every /plot subcommand, with its arguments, node and what it does.
icon: user
---

Every node here is given to everyone by default except `uxmplots.template.save`. Players only.

## Claiming and travel

| Command | Node | What it does |
|---|---|---|
| `/plot` | `uxmplots.info` | Opens the [panel](../menus/panel.md) of the plot you stand on |
| `/plot claim` | `uxmplots.claim` | Takes the plot you stand on |
| `/plot auto` | `uxmplots.claim` | Takes the nearest free plot and goes to it |
| `/plot price` | `uxmplots.claim` | What your next plot costs |
| `/plot abandon` | `uxmplots.abandon` | Gives a plot of yours back, asked twice |
| `/plot info` | `uxmplots.info` | Who owns the plot you stand on, and what it is |
| `/plot home [number]` | `uxmplots.home` | Goes to a plot of your own |
| `/plot tp <plot>` | `uxmplots.visit` | Goes to a plot by its id |
| `/plot visit <target>` | `uxmplots.visit` | Goes to somebody's plot, by player name or plot id |
| `/plot random` | `uxmplots.visit` | Goes to a plot you have not seen |
| `/plot slots` | `uxmplots.slots` | What you bought, hold and may hold |
| `/plot slots buy` | `uxmplots.slots` | Buys room for one more plot |

## Members and visitors

| Command | Node | What it does |
|---|---|---|
| `/plot members` | `uxmplots.trust` | Opens [who builds here and who is kept off](../menus/panel.md#members) |
| `/plot trust <player> [grades]` | `uxmplots.trust` | Lets somebody build; grades are `build`, `interact`, `container`, `settings`, separated by commas or spaces; none means build and interact |
| `/plot untrust <player>` | `uxmplots.trust` | Takes it away |
| `/plot ban <player>` | `uxmplots.ban` | Keeps somebody off |
| `/plot unban <player>` | `uxmplots.ban` | Lets them back in |
| `/plot visiting <policy>` | `uxmplots.visiting` | `open`, `trusted` or `closed` |
| `/plot session open <minutes>` | `uxmplots.session` | Opens a plot of yours for others to build on |
| `/plot session close` | `uxmplots.session` | Closes it early |
| `/plot session join` | `uxmplots.session` | Joins the session on the plot you stand on |
| `/plot session leave` | `uxmplots.session` | Leaves it |
| `/plot session` | `uxmplots.session` | The sessions that are open, as lines |
| `/plot sessions` | `uxmplots.session` | The [sessions window](../menus/community.md#sessions) |

## Settings

| Command | Node | What it does |
|---|---|---|
| `/plot name [name]` | `uxmplots.name` | Writes a name on a plot of yours; none clears it |
| `/plot flag <flag> [value]` | `uxmplots.flag` | Reads or sets one flag |
| `/plot flags` | `uxmplots.flag` | Every flag and what it does |
| `/plot tag <tag>` | `uxmplots.tag` | Puts a word from `tags.conf` on a plot of yours |
| `/plot tags` | `uxmplots.tag` | The words a plot wears |
| `/plot hide [value]` | `uxmplots.hide` | Keeps a plot of yours out of the gallery and off the boards |
| `/plot feel <kind> [choice]` | `uxmplots.feel` | What a plot sounds and looks like: music, ambience, time, weather, hologram |
| `/plot feel list <kind>` | `uxmplots.feel` | What one kind has on offer |

## Shaping

| Command | Node | What it does |
|---|---|---|
| `/plot floor <pattern>`, `/plot floor list` | `uxmplots.floor` | Lays one of the world's floors |
| `/plot wall <pattern>`, `/plot wall list` | `uxmplots.wall` | Builds the ring around the plot |
| `/plot border <pattern>`, `/plot border list` | `uxmplots.border` | Lays a border on top of the wall |
| `/plot ground <pattern>`, `/plot ground list` | `uxmplots.ground` | Fills what the plot is made of under its floor |
| `/plot biome <biome>`, `/plot biome list` | `uxmplots.biome` | Puts one of the world's biomes on the plot |
| `/plot clear` | `uxmplots.clear` | Takes down what was built, keeping the ground; asked twice |
| `/plot reset` | `uxmplots.reset` | Puts the plot back as generated; asked twice |
| `/plot snapshot` | `uxmplots.snapshot` | Takes a picture |
| `/plot snapshot list` | `uxmplots.snapshot` | The pictures you took |
| `/plot snapshot restore <name>` | `uxmplots.snapshot` | Puts one back; asked twice |
| `/plot template [name]` | `uxmplots.template` | Puts a layout the operator keeps down; none lists them |
| `/plot template save [as]` | `uxmplots.template.save` | Keeps a plot as a layout (operators) |
| `/plot merge <side>` | `uxmplots.merge` | Joins your plot with yours on that side: `north`, `east`, `south`, `west` |
| `/plot unmerge` | `uxmplots.merge` | Splits it again |
| `/plot outline` | `uxmplots.outline` | Draws the edge in the air for a few seconds |

## Gallery and community

| Command | Node | What it does |
|---|---|---|
| `/plot browse` | `uxmplots.browse` | Opens the [gallery](../menus/gallery.md) |
| `/plot rate <stars>` | `uxmplots.rate` | Rates the plot you stand in, 1 to 5 |
| `/plot rate part <part> <stars>` | `uxmplots.rate` | Rates one part, when `rating.categories` names parts |
| `/plot guestbook` | `uxmplots.guestbook` | Opens the plot's [guestbook](../menus/community.md#guestbook) |
| `/plot sign <note>` | `uxmplots.guestbook` | Leaves a line in it |
| `/plot tip <amount> [currency]` | `uxmplots.tip` | Tips the builder of the plot you stand in |
| `/plot tips` | `uxmplots.tip` | What a plot has been tipped |
| `/plot follow <player>`, `unfollow <player>` | `uxmplots.follow` | Follows a builder, or stops |
| `/plot mute <player>`, `unmute <player>` | `uxmplots.follow` | Keeps following without hearing, or hears again |
| `/plot following` | `uxmplots.follow` | Who you follow |
| `/plot chat` | `uxmplots.chat` | Points your chat at the people standing in this plot, or back at the server; it follows you from plot to plot and ends when you walk off |
| `/plot report <reason>` | `uxmplots.report` | Says something is wrong with a build |
| `/plot leaderboard` | `uxmplots.leaderboard` | Opens the [boards](../menus/gallery.md#boards) |
| `/plot leaderboard show <board>` | `uxmplots.leaderboard` | One board in full, in chat; console too |
| `/plot level` | `uxmplots.level` | How far a plot has climbed and what raised it |

## Market and shop

| Command | Node | What it does |
|---|---|---|
| `/plot sell <price> [currency]` | `uxmplots.sell` | Puts a plot of yours up for sale |
| `/plot sell cancel` | `uxmplots.sell` | Takes it off the market |
| `/plot market` | `uxmplots.sell` | What is for sale |
| `/plot rent [price] [currency] [days]` | `uxmplots.rent` | On your plot, puts it up to rent; on somebody else's, takes it for a period |
| `/plot rent cancel` | `uxmplots.rent` | Takes the rent sign down; anybody renting keeps their time |
| `/plot renting`, `/plot renting mine` | `uxmplots.rent` | What you rent and rent out |
| `/plot shop [kind]` | `uxmplots.shop` | Opens the [shop](../menus/shop.md), or lists one kind |
| `/plot buy` | `uxmplots.sell` | Buys the plot you stand on, when it is for sale |
| `/plot buy <kind> <entry>` | `uxmplots.shop` | Buys a shop entry, for good |
| `/plot owned` | `uxmplots.shop` | What you have bought |
| `/plot sponsor` | `uxmplots.sponsor` | What a sponsored place in the gallery costs |
| `/plot sponsor buy` | `uxmplots.sponsor` | Buys one for the plot you stand on |

## Language

| Command | Node | What it does |
|---|---|---|
| `/plot lang`, `lang set <language>`, `lang reset` | `uxmplots.language` | Your language |
| `/plot lang set-player <player> <language>` | `uxmplots.admin.language` | Another player's |
| `/plot lang reset-player <player>` | `uxmplots.admin.language` | Clear another player's |

Related: [Contest commands](contest.md), [Admin commands](admin.md)
