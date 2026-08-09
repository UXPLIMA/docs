---
title: Permission Reference
order: 1600
---

## Overview

Every uxmEssentials permission lives under the root `uxmessentials.*`. The plugin ships with defaults that make a fresh install playable the moment it starts, so you only grant permissions to *change* the defaults, never to switch the plugin on.

Two rules cover almost everything:

- **Self-service verbs default to `true`.** Read-only and own-account commands — `/home`, `/balance`, `/msg`, `/afk`, `/list`, `/warp`, `/kit`, `/vault`, `/help`, `/lang`, and their relatives — are available to everyone out of the box.
- **Staff verbs default to `op`.** Mutations, moderation, and anything that acts on another player — `/tp`, `/eco give`, `/ban`, `/vanish`, the item/world verbs, hologram and NPC admin — require operator status or an explicit grant.

A handful of marker and exempt nodes default to `false` (they *opt a player out* of something rather than granting an action). Those are called out where they appear.

<Callout type="info" title="LuckPerms is optional">

uxmEssentials never imports LuckPerms directly. Every check goes through the shared **`Permissions`** port, whose default implementation is the plain Bukkit `Permissible#hasPermission` call. That means every node on this page works against vanilla `op` and against any permission plugin (LuckPerms, GroupManager, and so on). The one place LuckPerms adds something is the **numbered / tiered nodes** below, which can *also* be expressed as LuckPerms meta — see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

</Callout>

To override a default for a group, configure the node in your permission plugin. uxmEssentials does not own group state.

---

## How nodes are organised

Permissions split along four axes:

| Axis | Shape | Typical default |
|------|-------|-----------------|
| **End-user** | `uxmessentials.home.use`, `uxmessentials.balance` | `true` |
| **Admin / staff** | `uxmessentials.<ctx>.admin`, `uxmessentials.admin.*` | `op` |
| **Bypass** | `uxmessentials.<feature>.<gate>.bypass` | `op` |
| **Numbered / tiered** | `uxmessentials.home.limit.<n>`, `uxmessentials.tp.warmup.<seconds>` | *(value-bearing, no boolean default)* |

A staff member usually holds the relevant admin node *and* the matching bypass node, so they keep operating through a gate — a cooldown, a warmup, a cost, a quota — that would otherwise reject them.

---

## Teleport

The `teleport` context owns all movement orchestration and the shared cooldown/warmup machinery; `homes`, `warps`, and `playerwarps` delegate execution here, so their gates layer on top.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.tpa.use` | `true` | `/tpa`, `/tpaccept`, `/tpdeny`, `/tpalist` — request and resolve teleports |
| `uxmessentials.tpahere.use` | `true` | `/tpahere <player>` — ask a player to come to you |
| `uxmessentials.tpa.cancel` | `true` | `/tpcancel` — withdraw your outgoing request |
| `uxmessentials.tpa.toggle` | `true` | `/tptoggle`, `/tpon`, `/tpoff` — refuse or accept incoming requests |
| `uxmessentials.tpa.auto` | `true` | `/tpauto` — auto-accept incoming requests |
| `uxmessentials.tpa.block` | `true` | `/tpblock` / `/tpunblock <player>` |
| `uxmessentials.tpa.all` | `op` | `/tpaall` — request every online player |
| `uxmessentials.tpsettings.use` | `true` | `/tpsettings` — your teleport settings panel |
| `uxmessentials.back.use` | `true` | `/back`, `/deathback` — return to your last location |
| `uxmessentials.back.ondeath` | `true` | Allow `/back`/`/deathback` to return to a **death** location |
| `uxmessentials.back.others` | `op` | `/back <player>` (staff) |
| `uxmessentials.rtp.use` | `true` | `/rtp` — random teleport from the pre-warmed safe queue |
| `uxmessentials.rtp.biome` | `true` | `/rtp biome <biome>` — random teleport into a chosen biome |
| `uxmessentials.rtp.gui` | `true` | `/rtp gui` — open the RTP menu |
| `uxmessentials.rtp.others` | `op` | `/rtp <player>` (staff) |
| `uxmessentials.rtp.cooldown.<seconds>` | — | Per-rank RTP cooldown (grant the number of seconds; highest wins) |
| `uxmessentials.rtp.radius.<n>` | — | Per-rank max RTP search radius (grant the number of blocks; highest wins) |
| `uxmessentials.teleport.settpr` | `op` | `/settpr <min> <max>` — set the live `/rtp` search zone |
| `uxmessentials.spawn.use` | `true` | `/spawn` — go to server spawn |
| `uxmessentials.spawn.named` | `true` | `/spawn <name>` — go to a named spawn |
| `uxmessentials.spawn.set` | `op` | `/setspawn`, `/setmainspawn`, `/removespawn`, `/mirrorspawn` |
| `uxmessentials.tp.use` | `op` | `/tp`, `/tphere`, `/goto`, `/bring` — direct staff teleport |
| `uxmessentials.tp.position` | `op` | `/tppos`, coordinate teleport |
| `uxmessentials.tp.offline` | `op` | `/tpoffline`, `/tpofflinehere` |
| `uxmessentials.tp.vertical` | `op` | `/top`, `/bottom`, `/jump`, `/up`, `/down`, `/ascend`, `/descend`, `/thru` |
| `uxmessentials.tp.all` | `op` | `/tpall` — pull every online player to you |
| `uxmessentials.tp.others` | `op` | Umbrella for the `[player]` form of staff teleport verbs |
| `uxmessentials.teleport.gui` | `op` | Teleport settings panel on the `/uxmess gui` hub |

---

## Homes

All home actions live under the single `/home` command; the no-arg call opens the slot grid and the rest are subcommands, each gated by its own node.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.home.use` | `true` | `/home` — open and manage your home grid |
| `uxmessentials.home.icon` | `true` | Pick a custom GUI icon for a home |
| `uxmessentials.home.visit` | `true` | `/home visit <player> [slot]` |
| `uxmessentials.home.invite` | `true` | `/home invite` / `/home uninvite` |
| `uxmessentials.home.admin` | `op` | `/home admin <player> …` — manage another player's homes (audited) |

---

## Warps

Single root `/warp`; create/move/delete/lock and the editor are subcommands.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.warp.use` | `true` | `/warp <name>` — teleport to a warp |
| `uxmessentials.warp.others` | `op` | `/warp <name> <player>` — send another player |
| `uxmessentials.warp.list` | `true` | `/warp list` |
| `uxmessentials.warp.info` | `true` | `/warp info <name>` |
| `uxmessentials.warp.set` | `op` | `/warp create` (alias `set`) |
| `uxmessentials.warp.move` | `op` | `/warp move <name>` |
| `uxmessentials.warp.delete` | `op` | `/warp del <name>` |
| `uxmessentials.warp.lock` | `op` | `/warp lock <name>` |
| `uxmessentials.warp.password` | `op` | `/warp password <name>` |
| `uxmessentials.warp.edit` | `op` | `/warp editor <name>` |
| `uxmessentials.warp.sign.create` | `op` | Create a `[warp]` sign |
| `uxmessentials.warp.sign.use` | `true` | Use a `[warp]` sign |

Per-warp gates are data-driven in `warps.conf`: a warp with `permission: true` requires the family node `uxmessentials.warp.use.<warp>` (`<warp>` is the warp id, lower-cased); a warp with a `cost` charges through the economy provider.

---

## Player Warps

Player-owned warps are keyed by `(owner, name)` — two players may each keep a `base`. The base nodes are self-service: holding one lets a player *run* the verb, while whether they may run it on a **particular** warp is decided by the owner/co-owner/manager role matrix inside the module. See [Player Warps](../features/player-warps.md).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.pwarp.use` | `true` | `/pwarp <name> [password]` and the browse GUI |
| `uxmessentials.pwarp.set` | `true` | `/setpwarp <name>` — create/re-anchor (capped by the limit tier) |
| `uxmessentials.pwarp.delete` | `true` | `/pwarp del <name>` |
| `uxmessentials.pwarp.list` | `true` | `/pwarps [player]` |
| `uxmessentials.pwarp.public` | `true` | `/pwarp visibility public\|private <name>` |
| `uxmessentials.pwarp.move` | `true` | `/pwarp move <name>` |
| `uxmessentials.pwarp.rename` | `true` | `/pwarp rename <name> <newName>` |
| `uxmessentials.pwarp.displayname` | `true` | `/pwarp displayname` |
| `uxmessentials.pwarp.description` | `true` | `/pwarp description` |
| `uxmessentials.pwarp.icon` | `true` | `/pwarp icon` |
| `uxmessentials.pwarp.category` | `true` | `/pwarp category` |
| `uxmessentials.pwarp.access` | `true` | `/pwarp access <name> PUBLIC\|PASSWORD\|WHITELIST\|PRIVATE` |
| `uxmessentials.pwarp.password` | `true` | `/pwarp password <name> <pw>\|clear` |
| `uxmessentials.pwarp.price` | `true` | `/pwarp price <name> <amount> [currency]` |
| `uxmessentials.pwarp.withdraw` | `true` | `/pwarp withdraw <name>` — collect banked entry fees |
| `uxmessentials.pwarp.info` | `true` | `/pwarp info <name>` |
| `uxmessentials.pwarp.rate` | `true` | `/pwarp rate <name> <1-5>` |
| `uxmessentials.pwarp.favourite` | `true` | `/pwarp favourite`, `/pwarp unfavourite` |
| `uxmessentials.pwarp.transfer` | `true` | `/pwarp transfer <name> <player>` |
| `uxmessentials.pwarp.members` | `true` | `/pwarp members <name> add\|remove` |
| `uxmessentials.pwarp.whitelist` | `true` | `/pwarp whitelist <name> add\|remove <player>` |
| `uxmessentials.pwarp.ban` | `true` | `/pwarp ban`, `/pwarp unban` |
| `uxmessentials.pwarp.sponsor` | `true` | `/pwarp sponsor <name> [days]` (also needs the sub-group enabled) |
| `uxmessentials.pwarp.admin` | `op` | The whole `/pwarp admin` group (restore, purge, delete, setowner, reload) |
| `uxmessentials.pwarp.gui` | `op` | Manage *every* player's warps in the `/pwarp` GUI |

The five bypasses (`uxmessentials.pwarp.bypass.ban`, `.password`, `.whitelist`, `.safety`, `.cost`) each default `op` and are listed under [Bypass Nodes](#bypass-nodes). The per-player cap is the tier `uxmessentials.pwarp.limit.<n>`, optionally scoped per world.

---

## Economy

Balances are DB-backed and routed through the shared `EconomyProvider` port; eco-admin mutations are always audit-logged.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.economy.balance` | `true` | `/balance [currency]` |
| `uxmessentials.economy.balance.others` | `op` | `/balance <player>` |
| `uxmessentials.economy.pay` | `true` | `/pay <player> <amount>` |
| `uxmessentials.economy.pay.toggle` | `true` | `/paytoggle` |
| `uxmessentials.economy.payall` | `op` | `/payall <amount>` |
| `uxmessentials.economy.baltop` | `true` | `/baltop` |
| `uxmessentials.economy.baltop.exempt` | `false` | Hide the holder from `/baltop` (admin / system accounts) |
| `uxmessentials.economy.worth` | `true` | `/worth [item]` |
| `uxmessentials.economy.sell` | `true` | `/sell`, `/sellall` |
| `uxmessentials.economy.setworth` | `op` | `/setworth [item] <price>\|clear` |
| `uxmessentials.economy.admin` | `op` | Umbrella for `/eco give`/`take`/`set` (audited) |
| `uxmessentials.economy.admin.give` | `op` | `/eco give` only |
| `uxmessentials.economy.admin.take` | `op` | `/eco take` only |
| `uxmessentials.economy.admin.set` | `op` | `/eco set` only |
| `uxmessentials.economy.admin.bulk` | `op` | `/eco giveall`, `/eco giverandom`, `/eco resetall` |

`/bank`, `/deposit`, `/withdraw`, `/loan`, `/wallet`, `/exchange`, `/sellall`, and the extra `/eco` subcommands carry their own nodes but are only active when the matching feature flag is on in `economy.conf`. Per-currency gating uses the tier node `uxmessentials.economy.currency.<id>` — see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Ranks

Rankup, prestige and the ladder panel. The plugin tracks each player's rank with a DB-backed pointer of its own; a rank's rankup actions may set a permission-plugin group, run any command, or nothing at all. The ladder (ranks, order, cost, requirements, actions) is data-driven in `modules/ranks/ranks.conf`.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.ranks.rankup` | `true` | `/rankup` — advance to the next rank when you meet its requirements |
| `uxmessentials.ranks.prestige` | `true` | `/prestige` — reset to the first rank for a prestige level (registered while prestige is enabled) |
| `uxmessentials.ranks.gui` | `true` | `/ranks` — open the self-service ladder panel (registered only when the GUI is enabled) |
| `uxmessentials.ranks.admin` | `op` | `/ranks setrank <player> <rank>` and the top-level `/setrank <player> <rank>` — set a player's rank directly (bypasses requirements, cost and actions) |

A rank's `cost` is charged through the economy provider, and a `placeholder` requirement uses an operator-chosen `%placeholder%` comparison — neither is a plugin-declared node.

---

## Kits

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.kit.use` | `true` | `/kit`, `/kit list`, `/kit <name>` — claim kits |
| `uxmessentials.kit.preview` | `true` | `/kit show <name>` |
| `uxmessentials.kit.others` | `op` | `/kit <name> <player>` |
| `uxmessentials.kit.edit` | `op` | `/kit create`/`del`/`editor` |
| `uxmessentials.kit.reset` | `op` | `/kit reset <player> [kit]` |

Per-kit gating is data-driven: a kit with `permission: true` requires `uxmessentials.kit.<id>`.

---

## Vaults

DB-persisted, player-owned item storage. Vault count and per-vault size are numbered quota tiers.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.vault.use` | `true` | `/vault`, `/vault <n>`, `/vault info`, `/vault delete <n>` (own) |
| `uxmessentials.vault.rename` | `true` | `/vault rename <n> [name]` |
| `uxmessentials.vault.icon` | `true` | `/vault icon <n> [material]` |
| `uxmessentials.vault.others` | `op` | `/vault <player> [n]` — open and audit another player's vault |
| `uxmessentials.vault.admin.delete` | `op` | `/vault delete <player> <n>` (audited, no refund) |
| `uxmessentials.vault.free` | `false` | Bypass every vault economy fee (no refund on delete) |

---

## Trade

Player-to-player trading through a shared, both-confirm window. `/trade` is the whole surface; the accept/deny answers and the (optional) cross-server escrow all ride on the one node.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.trade.use` | `true` | `/trade <player>` — request a trade; `/trade accept\|deny [player]` — answer a pending request |

The item blacklist, request distance, cooldown, staked currencies, audit, and cross-server escrow are config-driven in `modules/trade/config.conf`, not permission nodes.

---

## Player State

Toggleable flags and apply-once effects. Every `[player]` target form additionally requires `uxmessentials.playerstate.others`. These default to `op` because they are powerful self/other buffs — loosen the specific ones a donor rank should get.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.god.use` | `op` | `/god` |
| `uxmessentials.fly.use` | `op` | `/fly` |
| `uxmessentials.gamemode.use` | `op` | `/gamemode`, `/gmc`/`gms`/`gma`/`gmsp` |
| `uxmessentials.speed.use` | `op` | `/speed`, `/walkspeed`, `/flyspeed` |
| `uxmessentials.heal.use` / `.feed.use` | `op` | `/heal` / `/feed` |
| `uxmessentials.foodlevel.use` / `.health.use` | `op` | `/foodlevel` / `/health` — set a specific value |
| `uxmessentials.repair.use` / `.repair.all` | `op` | `/repair` / `/repairall` |
| `uxmessentials.extinguish.use` | `op` | `/ext` |
| `uxmessentials.clearinventory.use` | `op` | `/clearinventory` (`/ci`, `/clear`) |
| `uxmessentials.clearinventory.confirmtoggle` | `true` | `/citoggle` — self-clear confirmation toggle |
| `uxmessentials.invsee.use` (+`.invsee.modify`) | `op` | `/invsee` (modify unlocks editing) |
| `uxmessentials.endersee.use` | `op` | `/endersee` |
| `uxmessentials.suicide.use` | `true` | `/suicide` (self only) |
| `uxmessentials.near.use` | `op` | `/near [radius]` |
| `uxmessentials.nightvision.use` / `.glow.use` | `op` | `/nightvision` / `/glow` |
| `uxmessentials.ptime.use` / `.pweather.use` | `op` | Per-player time / weather |
| `uxmessentials.exp.use` | `op` | `/exp` (`/xp`) |
| `uxmessentials.air.use` / `.burn.use` / `.ice.use` | `op` | Air / fire / freeze |
| `uxmessentials.getpos.use` | `op` | `/getpos` (`/coords`, `/whereami`) |
| `uxmessentials.depth.use` `.biome.use` `.seed.use` `.compass.use` `.world.use` `.dimension.use` `.ping.use` | `true` | Read-only info verbs |
| `uxmessentials.playtime.use` / `.playtime.reset` | `op` | `/playtime` / reset |
| `uxmessentials.rest.use` | `op` | `/rest` |
| `uxmessentials.playerstate.others` | `op` | Use any of the above against a `[player]` target |

---

## Messaging

Private messages and persistent mail only (not public chat). Delivery is ignore-aware and respects a `moderation` mute.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.msg.use` | `true` | `/msg <player> <text>` |
| `uxmessentials.msg.reply` | `true` | `/reply <text>` |
| `uxmessentials.msg.toggle` | `true` | `/msgtoggle`, `/rtoggle` |
| `uxmessentials.msg.ignore` | `true` | `/ignore`, `/unignore`, `/ignorelist` |
| `uxmessentials.mail.use` | `true` | `/mail read\|send\|clear` |
| `uxmessentials.mail.sendall` | `op` | `/mail sendall <text>` (staff) |
| `uxmessentials.helpop.use` | `true` | `/helpop <text>` |
| `uxmessentials.helpop.receive` | `op` | Receive `/helpop` (staff side) |
| `uxmessentials.msg.color` | `op` | Render MiniMessage in PM/mail bodies |
| `uxmessentials.msg.socialspy` | `op` | `/socialspy` — observe others' PMs and mail |
| `uxmessentials.msgsettings.use` | `true` | `/msgsettings` |
| `uxmessentials.messaging.gui` | `op` | Messaging panel on the `/uxmess gui` hub |

---

## Communication

Connection-message policies, the rotating announcer, and info pages. Operator-authored bodies are config content; the nodes gate the *commands*.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.communication.broadcast` | `op` | `/broadcast <message>` |
| `uxmessentials.communication.broadcastworld` | `op` | `/broadcastworld` (`/bcw`) |
| `uxmessentials.communication.broadcasttoggle` | `true` | `/broadcasttoggle` — opt out of rotating announcements |
| `uxmessentials.communication.me` | `true` | `/me <action>` |
| `uxmessentials.communication.clearchat` | `op` | `/clearchat` (`/chatclear`) |
| `uxmessentials.communication.clearchat.exempt` | `false` | Keep your scrollback when staff run `/clearchat` |
| `uxmessentials.communication.togglechat` | `op` | `/togglechat` (`/mutechat`) |
| `uxmessentials.communication.chat.format` | `false` | Use MiniMessage formatting in your own public chat messages (only when `allow-player-format` is on) |
| `uxmessentials.announce.admin` | `op` | `/announce reload\|list\|preview\|toggle` |
| `uxmessentials.communication.gui` | `op` | Communication admin panel |
| `uxmessentials.communication.info.<name>` | `true` for shipped pages, `op` for custom | One node per info page — `/info`, `/rules`, `/motd`, and any operator-added page |

---

## Moderation

Every action is audit-logged and permission-gated. A mute blocks outbound messaging; a jail blocks `/home` and `/tpa`.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.moderation.mute` | `op` | `/mute`, `/tempmute`, `/mutehistory` |
| `uxmessentials.moderation.unmute` | `op` | `/unmute` |
| `uxmessentials.moderation.mutelist` | `op` | `/mutelist` |
| `uxmessentials.moderation.jail` | `op` | `/jail`, `/jails`, `/jailedplayers`, `/setjail`, `/jail del` |
| `uxmessentials.moderation.unjail` | `op` | `/unjail` |
| `uxmessentials.moderation.togglejail` | `op` | `/togglejail <player>` |
| `uxmessentials.moderation.ban` | `op` | `/ban`, `/unban`, `/banhistory` |
| `uxmessentials.moderation.banlist` | `op` | `/banlist` |
| `uxmessentials.moderation.tempban` | `op` | `/tempban` |
| `uxmessentials.moderation.banip` | `op` | `/banip`, `/tempbanip`, `/unbanip` |
| `uxmessentials.moderation.kick` | `op` | `/kick`, `/kickall` |
| `uxmessentials.moderation.warn` | `op` | `/warn`, `/tempwarn`, `/unwarn`, `/warns` |
| `uxmessentials.moderation.history` | `op` | `/history <player>` — unified disciplinary record |
| `uxmessentials.moderation.staffhistory` | `op` | `/staffhistory <staff>` |
| `uxmessentials.moderation.staffrollback` | `op` | `/staffrollback <staff> [limit]` |
| `uxmessentials.moderation.stats` | `op` | `/modstats [staff] [days]` — staff punishment analytics |
| `uxmessentials.moderation.templates` | `op` | `/punish <player> <template>` — apply a configured template |
| `uxmessentials.moderation.check` | `op` | `/checkban`, `/checkmute` |
| `uxmessentials.moderation.sanction` | `op` | `/sanction <player>` — read-only summary |
| `uxmessentials.moderation.freeze` | `op` | `/freeze`, `/unfreeze` |
| `uxmessentials.moderation.seen` | `op` | `/seen`, `/seenip`, `/alts` |
| `uxmessentials.moderation.commandspy` | `op` | `/commandspy` (`/cspy`) |
| `uxmessentials.moderation.lockdown` | `op` | `/lockdown [on\|off]` |
| `uxmessentials.moderation.sudo` | `op` | `/sudo <player> <command>` |
| `uxmessentials.moderation.gui` | `op` | `/mod` — moderation management GUI |
| `uxmessentials.moderation.exempt` | `op` | Target-side immunity from lower staff |
| `uxmessentials.moderation.broadcast.receive` | `op` | Receive the staff sanction broadcast |

Ban and mute durations can be capped per rank with the numbered families `uxmessentials.moderation.ban.maxduration.<seconds>` and `.mute.maxduration.<seconds>` — see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Staff

STAFF-MODE ONLY — a loadout swap and a gadget hotbar. It issues no sanctions of its own; the FREEZE and COMPASS gadgets orchestrate the moderation and teleport use cases. The `/staff` text roster and the `uxmessentials.staff.member` marker live under [Presence](#presence).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.staff.mode` | `op` | `/staffmode [player]` — enter staff mode (loadout swap + gadgets + vanish) |
| `uxmessentials.staff.chat` | `op` | `/staffchat` (`/sc`) — staff-only channel |
| `uxmessentials.staff.list` | `op` | `/stafflist` — online-staff GUI |

---

## Vanish

PremiumVanish-class invisibility with layered see/use levels. Its own `vanish` module — the single authority every other context reads for vanish state. See the [Vanish](../features/vanish.md) guide for the level rules.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.vanish.use` | `op` | `/vanish` (and `on`/`off`, `/vanish pickup`) — become invisible |
| `uxmessentials.vanish.others` | `op` | `/vanish <player>` — toggle another player's vanish |
| `uxmessentials.vanish.list` | `op` | `/vanish list` — the hidden players you may see (scoped to your see level) |
| `uxmessentials.vanish.see` | `op` | See vanished players (staff-among-staff) and `/tp` to them |
| `uxmessentials.vanish.silent` | `op` | `/vanish -s` — vanish or reappear with no fake join/quit broadcast |
| `uxmessentials.vanish.persist` | `op` | Rejoin already vanished across a relog |

The optional layered families `uxmessentials.vanish.use.level<N>` (vanish at use level `N`) and `uxmessentials.vanish.see.level<N>` (see up to level `N`) tier the see/use model — a viewer sees a vanished player only when their see level is at least the target's use level. Plain `.use` / `.see` are level 1; see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Presence

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.afk.use` | `true` | `/afk [reason]` |
| `uxmessentials.list.use` | `true` | `/list` |
| `uxmessentials.realname.use` | `true` | `/realname <player>` |
| `uxmessentials.nick.use` | `true` | `/nick <name>\|off` |
| `uxmessentials.nick.others` | `op` | `/nick <player> <name>` |
| `uxmessentials.whois.use` | `op` | `/whois <player>` |
| `uxmessentials.gc.use` | `op` | `/gc` (`/lag`, `/tps`, `/mem`) |
| `uxmessentials.staff.use` | `op` | `/staff` — list online staff |
| `uxmessentials.staff.member` | `op` | Marks a player as staff for `/staff` (a marker, not a command) |
| `uxmessentials.afk.kick.exempt` | `op` | Never auto-kicked for being AFK |
| `uxmessentials.presencesettings.use` | `true` | `/presencesettings` |
| `uxmessentials.presence.gui` | `op` | Presence panel on the `/uxmess gui` hub |

---

## Worlds

Multi-world management. Nodes use the `uxmessentials.world.*` prefix (the plural `/worlds` command; `playerstate`'s singular `/world` info verb also reads `uxmessentials.world.use`).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.world.use` | `true` | `/worlds` |
| `uxmessentials.world.list` / `.info` | `op` | `/worlds list` / `info` |
| `uxmessentials.world.create` / `.import` | `op` | `/worlds create` / `import` |
| `uxmessentials.world.load` / `.unload` / `.unregister` | `op` | Load / unload / unregister |
| `uxmessentials.world.delete` | `op` | `/worlds delete` + `/worlds confirm` |
| `uxmessentials.world.set` / `.gamerule` | `op` | Properties / gamerules |
| `uxmessentials.world.setspawn` | `op` | `/worlds setspawn` |
| `uxmessentials.world.spawn` | `true` | `/worlds spawn [name]` |
| `uxmessentials.world.tp` (+`.tp.others`) | `op` | Teleport self / another to a world |
| `uxmessentials.world.gui` | `op` | `/worlds gui` |
| `uxmessentials.world.pregen` | `op` | `/worlds pregen` |
| `uxmessentials.world.backup` / `.restore` | `op` | Backup / restore |

Per-world entry uses `uxmessentials.world.<name>.enter` (a world with access restrictions). Bypass nodes are listed under [Bypass Nodes](#bypass-nodes).

---

## Item & World

The item/world toolbox carries roughly 65 verbs, each with its own node; the full per-command list lives on the [Items, Blocks & World](../commands/items-world.md) command page. They default `op`, except the read-only inspection verbs (`/itemdb`, `/iteminfo`, `/recipe`) which are self-service. Two nodes are worth calling out here — the held-item editor, and the in-inventory shulker editor, which has no command of its own.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.itemworld.itemedit` | `op` | `/itemedit` — rename, re-lore, enchant, flag, attribute, durability, unbreakable and custom-model-data edits on the held item |
| `uxmessentials.itemworld.shulker` | `op` | Sneak-right-click a shulker box in the inventory to open and edit its contents in place |

`/give`, `/enchant` and `/spawnmob` also layer a per-type opt-out family — `uxmessentials.itemworld.give.<material>`, `.enchant.<enchant>`, `.spawnmob.<mob>` — see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes). The module's reload tier is `uxmessentials.module.itemworld`.

---

## Holograms

Named, world-placed native-Display holograms. The whole `/hologram` surface is gated as one operator tool.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.hologram.use` | `op` | `/hologram` — create, edit, move, style, restrict, and delete holograms |
| `uxmessentials.holograms.gui` | `op` | `/hologram` (no args) opens the management GUI; the holograms entry on the `/uxmess gui` hub |

A hologram gated with `visibility <name> PERMISSION <node>` uses an **operator-chosen** node (any node your permission plugin manages) — it is not a plugin-declared node and has no fixed entry.

---

## NPC

Server-wide packet fake-player/entity NPCs. The `/npc` command is an operator surface gated as a whole.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.npc.admin` | `op` | `/npc` — create, skin, equip, pose, bind actions, and manage NPCs |
| `uxmessentials.npc.use` | `true` | Interact with (click) an NPC. Reserved for a future per-NPC gate; the click currently runs for everyone |
| `uxmessentials.npc.gui` | `op` | `/npc` (no args) opens the management GUI; the npc entry on the `/uxmess gui` hub |

---

## Menus

The operator surface over the built-in [menu engine](../menus/engine.md).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.menu.use` | `true` | `/menu open <name>`, `/menu list`, `/menu last` |
| `uxmessentials.menu.open.others` | `op` | `/menu open <name> <target>` — open a menu for another player |
| `uxmessentials.menu.admin` | `op` | `/menu reload`, `/menu execute`, `/menu dump`, `/menu meta`, `/menu convert`, `/menu save` |
| `uxmessentials.menu.editor` | `op` | `/menu editor` — build and edit menus in-game |

---

## Vote

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.vote.use` | `true` | `/vote`, `/vote claim`, `/vote total\|streak`, reminders |
| `uxmessentials.voteparty.use` | `true` | `/voteparty` — progress to the next party |
| `uxmessentials.vote.top` | `true` | `/vote top [period]` |
| `uxmessentials.vote.testreward` | `op` | `/vote testreward` |
| `uxmessentials.vote.admin` | `op` | `/vote admin givevote\|reset` |
| `uxmessentials.voteparty.admin` | `op` | `/voteparty force\|set\|add` |

---

## Scoreboard, Tablist & Nametags

The **tablist** and **nametags** modules register no commands — both are packet/config-driven.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.scoreboard.use` | `true` | `/scoreboard` (`/sb`) — toggle whether you see the sidebar |
| `uxmessentials.scoreboard.gui` | `true` | `/scoreboard gui` — per-player settings panel |

---

## Discord Linking

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.discord.link` | `true` | `/discordlink`, `/discordlink status`, `/discordunlink` (own account only) |
| `uxmessentials.discord.gui` | `true` | `/discordlink gui` — per-player link-status panel |

The `/link` redemption itself runs in the optional Discord bridge jar; the in-game commands work whether or not the bridge is installed.

---

## Sitting & Poses

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.sit.use` | `true` | `/sit` and right-clicking a seat block to sit |
| `uxmessentials.playersit.use` | `true` | Right-click another player to sit on them (feature is off by default in config) |
| `uxmessentials.lay.use` | `true` | `/lay` |
| `uxmessentials.bellyflop.use` | `true` | `/bellyflop` |
| `uxmessentials.spin.use` | `true` | `/spin` |
| `uxmessentials.crawl.use` | `true` | `/crawl` |
| `uxmessentials.poses.toggle` | `true` | `/poses toggle` — allow or refuse other players sitting on you |
| `uxmessentials.poses.gui` | `true` | Open the `/poses` settings panel |
| `uxmessentials.poses.cooldown.bypass` | `op` | Skip the pose cooldown |

The optional numbered node `uxmessentials.poses.cooldown.<seconds>` sets an anti-spam wait between starting poses (an open value space, like `kit.cooldown.<seconds>` — see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes)). See the [Sitting & Poses](../features/poses.md) guide for how each pose works.

---

## Survival

Opt-in gameplay mechanics, each an independently toggleable sub-feature. A mechanic carries a **use** node `uxmessentials.survival.<mechanic>` (whether it acts for the player), and the seven with a personal `/command` toggle also carry `uxmessentials.survival.<mechanic>.toggle` (whether the player may switch it, from the command or the `/survival` panel). All are self-service (`default: true`) — the module itself ships off, and once an operator turns it on the mechanics act for everyone and a player opts out.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.survival.treefeller` | `true` | Tree-feller acts for you — break one log to fell the whole trunk |
| `uxmessentials.survival.treefeller.toggle` | `true` | `/treefeller` — switch your personal tree-feller on or off |
| `uxmessentials.survival.veinminer` | `true` | Veinminer acts for you — break one block to mine the connected vein |
| `uxmessentials.survival.veinminer.toggle` | `true` | `/veinminer` — switch your personal veinminer on or off |
| `uxmessentials.survival.farmprotect` | `true` | Farmland protection acts for you — you won't trample your crops |
| `uxmessentials.survival.farmprotect.toggle` | `true` | `/farmprotect` — switch your personal farmland protection on or off |
| `uxmessentials.survival.farmassist` | `true` | Right-click a mature crop to harvest and replant it, spending one seed |
| `uxmessentials.survival.autopickup` | `true` | Auto-pickup acts for you — mined drops go straight to your inventory |
| `uxmessentials.survival.autopickup.toggle` | `true` | `/autopickup` — switch your personal auto-pickup on or off |
| `uxmessentials.survival.autosmelt` | `true` | Auto-smelt acts for you — ores are smelted as you mine them |
| `uxmessentials.survival.autosmelt.toggle` | `true` | `/autosmelt` — switch your personal auto-smelt on or off |
| `uxmessentials.survival.autosell` | `true` | Auto-sell acts for you — priced drops are sold for coin as you mine |
| `uxmessentials.survival.autosell.toggle` | `true` | `/autosell` — switch your personal auto-sell on or off |
| `uxmessentials.survival.autotool` | `true` | Auto-tool acts for you — the best tool swaps to hand as you mine |
| `uxmessentials.survival.autotool.toggle` | `true` | `/autotool` — switch your personal auto-tool on or off |
| `uxmessentials.survival.gui` | `true` | `/survival` — open your personal survival mechanics settings panel |

Fast leaf decay, the anvil unlocker, one-player sleep and head drops are world-side effects with no permission node — they're governed only by their config blocks. See the [Survival Mechanics](../features/survival.md) guide for what each mechanic does.

---

## Security

Account security — an optional second factor a player enrols themselves, join verification, op-command protection, and a same-IP alt / ClientID guard. **Not** a login system: there's no password or first-join gate, and on an offline-mode server it waits for your login plugin rather than replacing it. The two self-service nodes ship `true`; the staff reads and the bypass default to `op`. See the [Account Security](../features/security.md) guide for the flows.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.security.2fa` | `true` | `/2fa` — set up, confirm or disable the authenticator second factor on your own account |
| `uxmessentials.security.pin` | `true` | `/pin` — set, change, remove or lock the numeric PIN second factor on your own account |
| `uxmessentials.security.pin.required` | `false` | Holders **must** have a PIN: a holder with no factor is shown the create pad on join and cannot play until they set one |
| `uxmessentials.security.admin` | `op` | `/security` — inspect and manage another player's second factors. Gates the whole tree |
| `uxmessentials.security.bypass` | `op` | Exempt from the join-verification freeze and the op-command re-auth checks |
| `uxmessentials.security.force` | `op` | `/security force <player>` — make a player re-verify their second factor on their next action or join |
| `uxmessentials.security.reset` | `op` | `/security reset <player> [totp\|pin\|all]` — clear a factor a player can no longer prove |
| `uxmessentials.security.alts` | `op` | `/ipalts <player>` — list the accounts that share an IP with a player |
| `uxmessentials.security.clientinfo` | `op` | `/clientinfo <player>` — show the client brand a player reported |
| `uxmessentials.security.alts.notify` | `op` | Receive the staff notice when a join shares an IP with other accounts or reports a flagged client |

---

## Command Control

Command whitelist / blacklist gating, tab-completion filtering, plugin-hide, and the namespace-bypass block — the PlHidePro / CommandWhitelist feature set. It ships enabled but inert (a blacklist with empty lists, plugin-hide off), so both nodes default to `op` and gate nobody until you name commands. See the [Command Control](../features/commandcontrol.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.commandcontrol.bypass` | `op` | Exempt from the whitelist/blacklist gate and the tab-completion / plugin-hide scrub (namespaced forms included) |
| `uxmessentials.commandcontrol.viewplugins` | `op` | See the plugin-listing / help commands (`/plugins`, `/pl`, `/help`, ...) hidden by plugin-hide |
| `uxmessentials.commandcontrol.channelhide.bypass` | `op` | Exempt from the plugin-channel hider — the full channel-registration list is sent to this player |
| `uxmessentials.commandcontrol.spam.bypass` | `op` | Exempt from the command-spam rate limiter — commands are never counted and no spam action fires |

---

## Villagers

Villager trade management — trades that never lock out, a restock timer, a staff trade editor, click-to-trade, a villager saver, pickup, follow, and leashing. The module ships **disabled**, so all six action nodes default to `op` and grant nothing until you turn the module on and then the matching feature with it. The four trade-availability switches (infinite trading, restock, instant restock, disable trades) take no permission — they're config-only. See the [Villagers](../features/villagers.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.villagers.use` | `op` | `/villager` — the root command's base node; each subcommand gates further on its own node below |
| `uxmessentials.villagers.manager` | `op` | `/villager manager` — open and edit the trades of the villager you're looking at |
| `uxmessentials.villagers.trade` | `op` | Open a villager's trade window directly on right-click, when click-to-trade is enabled |
| `uxmessentials.villagers.protect` | `op` | `/villager protect` — toggle whether the villager you're looking at is protected from death and despawn |
| `uxmessentials.villagers.bucket` | `op` | Sneak-right-click a villager to pick it up into a captured-villager item, and place it back later |
| `uxmessentials.villagers.follow` | `op` | `/villager follow` — toggle whether the villager you're looking at pathfinds after you |
| `uxmessentials.villagers.leash` | `op` | Right-click a villager with a lead to leash it, when leashing is enabled |

---

## Inventory Rollback

Inventory snapshot and restore — the plugin freezes a player's inventory on death and (by config) on logout, keeps a bounded history of those snapshots in the database (never PDC, so they survive a world rollback), and lets staff browse and restore them from a GUI. The module ships **disabled**; turn it on and it captures on death and logout straight away. A restore acts on a live inventory, so the target must be online. See the [Inventory Rollback](../features/invrollback.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.invrollback.restore` | `op` | `/invrestore <player>` — open the inventory-snapshot restore GUI, then preview and restore one of that player's snapshots |
| `uxmessentials.invrollback.export` | `op` | `/invrestore export <player> <index>` — pack a snapshot into shulker boxes and hand them to you |
| `uxmessentials.invrollback.teleport` | `op` | `/invrestore tp <player> <index>` — teleport to where the snapshot was captured |

---

## Regions

WorldGuard region management — a menu over an installed WorldGuard to browse regions,
define cuboids, cycle their state flags, and edit their member/owner rosters. WorldGuard is
a **soft-dependency**: with it absent the module is inert and every command answers
*"WorldGuard not installed"* (WorldEdit is a further optional soft-depend, used only for
selection-based `/regions create`). Region management is staff work, so all five action
nodes default to `op`. See the [Regions](../features/regions.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.regions.list` | `op` | `/regions [world]` — browse a world's regions and open a region's detail panel |
| `uxmessentials.regions.create` | `op` | `/regions create <id>`, `/regions pos1`, `/regions pos2` — define a new cuboid region |
| `uxmessentials.regions.flags` | `op` | The flag editor (`/regions flags <id>`) — cycle a region's state flags `UNSET → ALLOW → DENY` |
| `uxmessentials.regions.members` | `op` | The member/owner roster (`/regions members <id>`), plus `/regions addmember` / `addowner` |
| `uxmessentials.regions.admin` | `op` | `/regions priority <id> <value>` — set a region's priority |

---

## Server Tweaks

A grab-bag of silent server-side infrastructure tweaks — a custom F3 brand, a console-log
filter, an unsigned-chat option, and a SignedVelocity backend companion. The module is
**config-only**: it registers no commands, so it has **no action permission nodes**. Every
tweak defaults off and is switched entirely from `modules/servertweaks/config.conf`. See
the [Server Tweaks](../features/server-tweaks.md) guide. The module's reload tier is
`uxmessentials.module.servertweaks`.

---

## Shared kernel

The kernel owns no feature commands, but carries two cross-cutting self-service verbs.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.lang.use` | `true` | `/lang [code\|reset]` — your personal language override |
| `uxmessentials.help` | `true` | `/help [page\|query]` — list the commands you can run (per-line permission-filtered) |

---

## Numbered and Tiered Nodes

Some limits are *values*, not yes/no flags: how many homes a player may own, how long their teleport cooldown is. Instead of hard-coding these per group, uxmEssentials encodes the value **in the node** and reads the best-matching one the player holds, falling back to a config default.

| Node family | Meaning | Reduction rule |
|-------------|---------|----------------|
| `uxmessentials.home.limit.<n>` | Home quota | **max** — highest `<n>` wins |
| `uxmessentials.vault.amount.<n>` | Vault count | **max** |
| `uxmessentials.vault.size.<rows>` | Per-vault rows (1–6) | **max** |
| `uxmessentials.pwarp.limit.<n>` | Player-warp quota | **max** |
| `uxmessentials.tp.warmup.<seconds>` | Teleport warmup | **min** — lowest wins; `0` removes the warmup |
| `uxmessentials.tp.cooldown.<seconds>` | Teleport cooldown | **min** |
| `uxmessentials.warp.cooldown.<seconds>` | Warp cooldown | **min** |
| `uxmessentials.kit.cooldown.<seconds>` | Per-kit cooldown | **min** |
| `uxmessentials.warp.use.<warp>` | Per-warp gate (when the warp sets `permission: true`) | boolean |
| `uxmessentials.kit.<id>` | Per-kit gate (when the kit sets `permission: true`) | boolean |
| `uxmessentials.economy.currency.<id>` | Per-currency gate (when `permission-required = true`) | boolean |
| `uxmessentials.itemworld.give.<material>` / `.enchant.<enchant>` / `.spawnmob.<mob>` | Per-type opt-out (wildcard parents default `true`) | boolean, opt-out |
| `uxmessentials.moderation.ban.maxduration.<seconds>` / `.mute.maxduration.<seconds>` | Max sanction duration a holder may issue | **max**; none = unlimited |
| `uxmessentials.vanish.use.level<N>` / `.see.level<N>` | Vanish use level / see level (a viewer sees a target when see ≥ use) | **max** — highest `<N>` wins |
| `uxmessentials.communication.info.<name>` | Per info-page gate | boolean |

### How they resolve

Every value-bearing node passes through the shared **`Permissions`** port and a single **`QuotaNodeReducer`**, so the semantics are identical across contexts:

- **Quota families** (`home.limit`, `vault.amount`, `vault.size`, `pwarp.limit`, `*.maxduration`) reduce to the **maximum** — more is better, the most generous node wins.
- **Cooldown / warmup families** (`*.cooldown`, `*.warmup`) reduce to the **minimum** — less is better; `0` means "no wait".
- **`-1` is the unlimited sentinel** for quota families (a `home.limit.-1` means no quota at all). It is never used for cooldown/warmup families, where `0` is "no wait".
- A player with **no** numbered node and no meta falls back to the per-context **config default** — never to "unlimited".

A numbered node may carry an **optional `<world>` segment** between the family and the value, so a quota or cooldown can differ per destination world:

```
uxmessentials.home.limit.creative.20
uxmessentials.tp.warmup.nether.0
uxmessentials.warp.cooldown.<world>.<seconds>
```

The unscoped form applies everywhere; the world-scoped form applies only when that world is the one being resolved against. Matching scoped and unscoped nodes are simply folded into the same reducer.

<Callout type="note" title="LuckPerms meta is an alternative, never a requirement">

When LuckPerms is installed, these same quotas can also be expressed as LuckPerms **meta** (for example `meta.home-limit`, `meta.warp-cooldown`). A meta value takes precedence over the numbered node when present. With no permission plugin, only the numbered node and the config default apply — the numbered form is the portable baseline.

</Callout>

Because the value spaces (`<n>`, `<seconds>`, `<rows>`, and so on) are open, these nodes are **not** enumerated in `paper-plugin.yml`; operators grant the specific instances they need.

---

## Bypass Nodes

Bypass nodes let a trusted player cut through a gate — a cooldown, warmup, cost, lock, or restriction — that would normally reject them. They all default to `op`, except the two vault/marker exempt nodes noted below.

| Node | What it bypasses |
|------|------------------|
| `uxmessentials.tp.warmup.bypass` | Teleport warmup (immune to move-cancel) |
| `uxmessentials.tp.cooldown.bypass` | The shared teleport cooldown |
| `uxmessentials.home.bypass.unsafe` | The unsafe-destination confirm on a home teleport |
| `uxmessentials.home.bypass.cost` | The per-action economy cost for home create/relocate/teleport |
| `uxmessentials.warp.bypass.lock` | A locked warp |
| `uxmessentials.warp.bypass.password` | A password-protected warp |
| `uxmessentials.warp.bypass.safety` | A warp whose destination fails the safety check |
| `uxmessentials.warp.cooldown.bypass` | The warp teleport cooldown |
| `uxmessentials.warp.cost.bypass` | The optional per-warp cost |
| `uxmessentials.pwarp.bypass.ban` | A player warp you are banned from |
| `uxmessentials.pwarp.bypass.password` | A password-protected player warp |
| `uxmessentials.pwarp.bypass.whitelist` | A whitelist-only player warp |
| `uxmessentials.pwarp.bypass.safety` | An unsafe player-warp destination |
| `uxmessentials.pwarp.bypass.cost` | A player warp's entry price |
| `uxmessentials.economy.pay.confirm.bypass` | The `/payconfirm` prompt above the confirm threshold |
| `uxmessentials.economy.tax.bypass` | The `/pay` tax cut (renameable via `pay.tax.bypass-permission`) |
| `uxmessentials.economy.bypasscmdcost` | The configured per-command economy charge |
| `uxmessentials.vault.bypass-blacklist` | The vault material blacklist |
| `uxmessentials.vault.free` *(default `false`)* | Every vault economy fee (no refund on delete) |
| `uxmessentials.msg.bypass.ignore` | A player who has ignored you |
| `uxmessentials.moderation.lockdown.bypass` | The `/lockdown` login refusal |
| `uxmessentials.communication.chat.bypass` | A `/togglechat` chat lock |
| `uxmessentials.cooldown.bypass.<label>` | A generic per-command cooldown registered under `<label>` |
| `uxmessentials.world.access.bypass` | Per-world entry restrictions on every world |
| `uxmessentials.world.gamemode.bypass` | A world's forced gamemode |
| `uxmessentials.world.command-bypass` | The per-world command blocker |
| `uxmessentials.playerstate.fly.allworlds` | The no-fly-world restriction on `/fly` |

---

## Module Gates and Admin Roots

### `uxmessentials.module.<id>` — per-module reload tier

Every bounded context is a first-class feature module. Each owns a node tier `uxmessentials.module.<id>` (default `op`) — **not** the feature's runtime permission, but the per-module **administrative** gate: who may inspect and hot-reload that one module via `/uxmess reload <id>` without holding the blanket `uxmessentials.admin.reload`. `<id>` is the module id used everywhere else (config path `modules.<id>`, the reload command, the tier node):

```
teleport   worlds     homes       economy    warps      kits
playerstate messaging presence    moderation itemworld  vaults
communication holograms playerwarps scoreboard tablist  nametags
vote       discordlink staff       npc        custommenus
poses      survival    ranks       trade      vanish
security   commandcontrol villagers   invrollback
regions    servertweaks
```

`/uxmess reload <module>` accepts either `uxmessentials.admin.reload` (blanket) or the matching `uxmessentials.module.<id>` (delegated), so you can hand a junior staffer reload rights over one module without the keys to the whole plugin.

### `/uxmess` admin tree

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.admin` | `op` | `/uxmess` read-only subcommands: `status`, `doctor`, help |
| `uxmessentials.admin.reload` | `op` | `/uxmess reload` (all or one module) |
| `uxmessentials.admin.import` | `op` | `/uxmess import <source>` — the one-shot idempotent importer |
| `uxmessentials.admin.backup` | `op` | `/backup` — snapshot the plugin data directory |
| `uxmessentials.gui` | `op` | `/uxmess gui` — the central management hub |

### Management-GUI nodes

The management hub gathers each module's management GUI behind one menu. Each module registers its entry under `uxmessentials.<module>.gui` (for example `uxmessentials.holograms.gui`, `uxmessentials.npc.gui`, `uxmessentials.economy.admin`). Each defaults to `op`, the hub shows a viewer only the entries whose node they hold, and a module's own `/<module>` GUI entry point opens directly for a holder of the same node — so you can delegate one module's panel without granting the rest.

---

<Callout type="tip" title="Granting with LuckPerms">

All of these are ordinary permission nodes. To hand a `donor` group extra homes and free flight, for example:

```
/lp group donor permission set uxmessentials.home.limit.10 true
/lp group donor permission set uxmessentials.fly.use true
/lp group donor permission set uxmessentials.tp.warmup.bypass true
```

Restrict a single per-type node by **negating** it (the opt-out families default `true`):

```
/lp group default permission set uxmessentials.itemworld.spawnmob.wither false
```

Or express a quota as meta instead of a numbered node:

```
/lp group donor meta set home-limit 10
```

</Callout>

---

## Next Steps

- [Command Overview](../commands/overview.md) — how commands are registered, aliased, renamed, and disabled
- [LuckPerms Integration](../integrations/luckperms.md) — meta-based quotas and group resolution
- [Developer API](../developer/overview.md) — integrate with uxmEssentials at runtime
