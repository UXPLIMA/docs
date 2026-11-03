---
title: Admin commands
order: 204
description: Every /uxmplots subcommand, with its arguments, node and what it does.
icon: shield
---

Every node here is operators only by default and hides its word from anyone who lacks it.

| Command | Node | What it does |
|---|---|---|
| `/uxmplots`, `/uxmplots version` | `uxmplots.admin.version` | The version this server runs |
| `/uxmplots reload` | `uxmplots.admin.reload` | Reads the configuration again |
| `/uxmplots doctor` | `uxmplots.admin.doctor` | One line per check |
| `/uxmplots reports` | `uxmplots.admin.reports` | Opens the [report queue](../menus/staff.md) |
| `/uxmplots report <decision> <plot>` | `uxmplots.admin.report` | Decides one: `valid`, `cleaned` or `unfounded` |
| `/uxmplots staff` | `uxmplots.admin.reports` | Opens the [staff bench](../menus/staff.md#bench) over the plot you stand on |
| `/uxmplots inactive` | `uxmplots.admin.inactive` | Every plot whose owner stopped coming |
| `/uxmplots reset` | `uxmplots.admin.reset` | Puts any plot back as generated |
| `/uxmplots snapshot` | `uxmplots.admin.snapshot` | Takes a picture of any plot |
| `/uxmplots owner <player>` | `uxmplots.admin.owner` | Hands the plot you stand on to somebody else |
| `/uxmplots delete` | `uxmplots.admin.delete` | Takes any plot back from its owner; the blocks stay |
| `/uxmplots unlock` | `uxmplots.admin.unlock` | Lets go of a contest lock on a plot, or puts it back |
| `/uxmplots redstone` | `uxmplots.admin.redstone` | Switches a plot's redstone back on before its pause runs out |
| `/uxmplots pin`, `/uxmplots unpin` | `uxmplots.admin.pin` | Puts the build you stand on in the gallery's pinned row, or takes it off |
| `/uxmplots phase <contest>` | `uxmplots.admin.phase` | Ends the phase a contest is in, so the next starts now |
| `/uxmplots board plant <board> <name>` | `uxmplots.admin.board` | Plants a board where you stand |
| `/uxmplots board uproot <name>` | `uxmplots.admin.board` | Takes one down |
| `/uxmplots world <world>` | `uxmplots.admin.world` | Opens the [world editor](../features/editors.md) |
| `/uxmplots world list` | `uxmplots.admin.world` | Every plot world this server runs |
| `/uxmplots pregen start <world>` | `uxmplots.admin.pregen` | Starts a world generating ahead again |
| `/uxmplots pregen stop <world>` | `uxmplots.admin.pregen` | Stops it until the server restarts |

`reload`, `version`, `doctor`, `reports`, `inactive`, `world list` and `pregen` run from the console; the rest act on
where the operator stands.

Related: [Expiry and staff](../features/expiry-and-staff.md)
