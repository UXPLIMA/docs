---
title: commands.conf
order: 603
description: The name, aliases, words per language and switch of every command.
icon: terminal
---

The words of /island and /isadmin.

Every word of the island command is yours. Change a name, add aliases, or turn a word off. The key in front of each block is the word the plugin ships; the name inside it is the word players type.

```text
name     the word players type
aliases  more words that do the same
enabled  false leaves the word out of the command entirely
```

localized-aliases words for the readers of one language, a list per language tag:

```text
                 localized-aliases { tr = ["arsa"] }
                 The tag is a language (tr) or a language and a country (pt-BR).
                 A word on a command works for anyone who types it, but only a
                 player who reads that language finds it among their commands.
                 A word under subcommands is shown to, and works for, the readers
                 of that language and the console only. The language a player
                 reads is their client's, or the server's own when config.conf
                 sets language.follow-client = false.
```

A name another branch already answers to is refused with a warning in the console, and that branch keeps its own word. A change needs a restart.

The file is `plugins/uxmSkyblock/commands.conf`. Every default below is the shipped value.

## `commands.island`

| Key | Default | What it does |
|---|---|---|
| `name` | `island` |  |
| `aliases` | `is` |  |
| `enabled` | `true` |  |

## `commands.island.subcommands.ac.accept.activity.admin.alliance.ally.allychat.ban.bank.bans.biome.bookmarks.booster.border.bounds.c.challenges.chat.chest.create.delete.delhome.deny.disband.doctor.end.explore.flag.go.gohome.guestbook.help.home.homes.info.invite.kick.lang.leave.level.limits.lock.lore.members.menu.missions.nether.oneblock.chunks.course.coursetag.courserate.courses.plot.contest.fly.plots.permissions.profile.quarantine.rate.recalc.reload.rename.reset.restore.rewards.role.season.sethome.setspawn.schematic.settings.shop.spy.top.trade.cargo.sail.market.contracts.plunder.boxbuy.boxside.trust.trusted.unban.unlock.untrust.upgrade.upgrades.value.vault.visit.values.visitfee.warp.warplock.warps.isadmin`

What an operator does to the islands of one mode. A branch is there for each mode the server runs, and each verb asks its own permission: uxmskyblock.admin.`<mode>`.`<verb>`, as uxmskyblock.admin.oneblock.setphase. The mode words and the verbs are renamed the same way.

| Key | Default | What it does |
|---|---|---|
| `name` | `isadmin` |  |
| `aliases` | empty list |  |
| `enabled` | `true` |  |
| `subcommands.oneblock.name` | `oneblock` |  |
| `subcommands.oneblock.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.name` | `chunkblock` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.name` | `tradewinds` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.name` | `realms` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.name` | `parkour` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.name` | `brix` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.name` | `loot` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.top.name` | `top` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.top.aliases` | empty list |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.top.subcommands.add.boxed.name` | `boxed` |  |
| `subcommands.oneblock.subcommands.setphase.setcount.addcount.chunkblock.subcommands.open.tradewinds.subcommands.setrank.realms.parkour.brix.subcommands.loot.top.subcommands.add.boxed.aliases` | empty list |  |
