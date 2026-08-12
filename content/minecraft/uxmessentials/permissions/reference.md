---
title: Permission Reference
order: 1600
---

## Overview

Every uxmEssentials permission lives under the root `uxmessentials.*`. The plugin ships with defaults that make a fresh install playable the moment it starts, so you only grant permissions to *change* the defaults, never to switch the plugin on.

Two rules cover almost everything:

- **Self-service verbs default to `true`.** Read-only and own-account commands: `/home`, `/balance`, `/msg`, `/afk`, `/list`, `/warp`, `/kit`, `/vault`, `/help`, `/lang`, and their relatives: are available to everyone out of the box.
- **Staff verbs default to `op`.** Mutations, moderation, and anything that acts on another player: `/tp`, `/eco give`, `/ban`, `/vanish`, the item/world verbs, hologram and NPC admin: require operator status or an explicit grant.

A handful of marker and exempt nodes default to `false` (they *opt a player out* of something rather than granting an action). Those are called out where they appear.

<Callout type="info" title="LuckPerms is optional">

uxmEssentials never imports LuckPerms directly. Every check goes through the shared **`Permissions`** port, whose default implementation is the plain Bukkit `Permissible#hasPermission` call. That means every node on this page works against vanilla `op` and against any permission plugin (LuckPerms, GroupManager, and so on). The one place LuckPerms adds something is the **numbered / tiered nodes** below, which can *also* be expressed as LuckPerms meta, see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

</Callout>

To override a default for a group, configure the node in your permission plugin. uxmEssentials does not own group state.

<Callout type="info" title="The plugin can print this page for you">

Every node below is declared once inside the plugin, in a single catalogue. The server is told about
the fixed nodes from that catalogue on enable, and the same catalogue answers `/uxmess permissions`
in game. `/uxmess permissions export` writes the whole thing to `permissions.md` in the plugin
folder, so the reference always matches the build you are running. Both need
`uxmessentials.admin.permissions`.

</Callout>

---

## How nodes are organised

Permissions split along four axes:

| Axis | Shape | Typical default |
|------|-------|-----------------|
| **End-user** | `uxmessentials.home.use`, `uxmessentials.balance` | `true` |
| **Admin / staff** | `uxmessentials.<ctx>.admin`, `uxmessentials.admin.*` | `op` |
| **Bypass** | `uxmessentials.<feature>.<gate>.bypass` | `op` |
| **Numbered / tiered** | `uxmessentials.home.limit.<n>`, `uxmessentials.tp.warmup.<seconds>` | *(value-bearing, no boolean default)* |

A staff member usually holds the relevant admin node *and* the matching bypass node, so they keep operating through a gate (a cooldown, a warmup, a cost, a quota) that would otherwise reject them.

---

## Teleport

The `teleport` context owns all movement orchestration and the shared cooldown/warmup machinery; `homes`, `warps`, and `playerwarps` delegate execution here, so their gates layer on top.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.tpa.all` | `op` | `/tpaall` to request every online player to teleport to you. |
| `uxmessentials.tpa.auto` | `true` | `/tpauto` to auto-accept incoming teleport requests. |
| `uxmessentials.tpa.block` | `true` | `/tpblock` / `/tpunblock` to block a player's requests. |
| `uxmessentials.tpa.cancel` | `true` | `/tpcancel` / `/tpacancel` to withdraw your outgoing request. |
| `uxmessentials.tpa.toggle` | `true` | `/tptoggle` to refuse all incoming teleport requests. |
| `uxmessentials.tpa.use` | `true` | `/tpa`, `/tpaccept`, `/tpdeny` to request and resolve a teleport. |
| `uxmessentials.tpahere.use` | `true` | `/tpahere` to ask a player to come to you. |
| `uxmessentials.tpsettings.use` | `true` | `/tpsettings` opens your personal teleport settings panel. |
| `uxmessentials.back.ondeath` | `true` | Allow `/back` and `/deathback` to return to a death location. |
| `uxmessentials.back.use` | `true` | `/back` to return to your last captured location; `/deathback` (alias `/dback`) to return to your last death location. |
| `uxmessentials.module.teleport` | `op` | Hot-reload / inspect the teleport module (`/tp`, `/tpa`, `/back`, `/spawn` and `/rtp`). |
| `uxmessentials.rtp.biome` | `true` | `/rtp` biome `<biome>` to random teleport into a specific biome. |
| `uxmessentials.rtp.gui` | `true` | `/rtp` gui to open the random-teleport world picker. |
| `uxmessentials.rtp.others` | `op` | `/rtp <player>` to force another online player to random teleport (staff). |
| `uxmessentials.rtp.radius.<blocks>` | `quota` | How far from the world centre `/rtp` may drop you; the largest tier held wins. |
| `uxmessentials.rtp.use` | `true` | `/rtp` random teleport from the pre-warmed safe-location queue. |
| `uxmessentials.spawn.named` | `true` | `/spawn <name>` to teleport to a named spawn. |
| `uxmessentials.spawn.set` | `op` | `/setspawn`, `/setmainspawn`, `/removespawn` and `/mirrorspawn` to define and manage spawns. |
| `uxmessentials.spawn.use` | `true` | `/spawn` to teleport to the resolved server spawn. |
| `uxmessentials.teleport.gui` | `op` | Show the teleport settings panel on the `/uxmess` gui hub. |
| `uxmessentials.teleport.settpr` | `op` | `/settpr <minRange> <maxRange>` to set the `/rtp` search zone at runtime. |
| `uxmessentials.tp.all` | `op` | `/tpall` to pull every online player to you. |
| `uxmessentials.tp.offline` | `op` | `/tpoffline` / `/tpofflinehere` to a player's logout location. |
| `uxmessentials.tp.others` | `op` | `/tpo` and `/tpohere` to teleport overriding no-tp flags. |
| `uxmessentials.tp.position` | `op` | `/tppos` to teleport to raw coordinates. |
| `uxmessentials.tp.use` | `op` | `/tp`, `/tphere`, `/goto`, `/bring` and `/tprandomplayer` (`/tprp`) direct staff teleport. |
| `uxmessentials.tp.vertical` | `op` | `/top`, `/bottom`, `/jump`, `/up`, `/down`, `/ascend`, `/descend`, `/thru` vertical teleports. |
| `uxmessentials.tp.warmup.<seconds>` | `tier` | The stand-still countdown before a teleport runs, in seconds; the shortest tier held wins and 0 removes it. |
| `uxmessentials.tp.warmup.bypass` | `op` | Start teleports with no warmup, immune to move-cancel. |

---

## Homes

All home actions live under the single `/home` command; the no-arg call opens the slot grid and the rest are subcommands, each gated by its own node.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.home.admin` | `op` | `/homeadmin` to manage another player's homes. |
| `uxmessentials.home.bypass.cost` | `op` | Skip the per-action economy cost for home create, relocate, and teleport actions. |
| `uxmessentials.home.bypass.unsafe` | `op` | Skip the unsafe-destination confirm when teleporting to a home via the GUI. |
| `uxmessentials.home.icon` | `true` | Pick a custom GUI icon for one of your homes from the grid. |
| `uxmessentials.home.invite` | `true` | `/invite` and `/uninvite` to grant or revoke another player's access to one of your homes. |
| `uxmessentials.home.limit.<n>` | `quota` | How many homes you may keep; the largest tier held wins. |
| `uxmessentials.home.use` | `true` | `/home` to open and manage your slot-based home grid. |
| `uxmessentials.home.visit` | `true` | `/visit` to teleport to another player's public home or one you were invited to. |
| `uxmessentials.module.homes` | `op` | Hot-reload / inspect the homes module (per-player homes and the slot grid). |

---

## Warps

Single root `/warp`; create/move/delete/lock and the editor are subcommands.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.warps` | `op` | Hot-reload / inspect the warps module (server warps and their access rules). |
| `uxmessentials.warp.bypass.lock` | `op` | Use a locked warp regardless of its lock state. |
| `uxmessentials.warp.bypass.password` | `op` | Use a password-protected warp without entering its password. |
| `uxmessentials.warp.bypass.safety` | `op` | Use a warp whose destination fails the safety check. |
| `uxmessentials.warp.delete` | `op` | `/delwarp <name>` to remove a warp. |
| `uxmessentials.warp.edit` | `op` | `/warp` editor `<name>` to open the warp editor (cost, gates, effects, welcome message, icon). |
| `uxmessentials.warp.info` | `true` | `/warpinfo <name>` to show a warp's owner, creation time and cost. |
| `uxmessentials.warp.list` | `true` | `/warps` to list the warps you may use. |
| `uxmessentials.warp.lock` | `op` | `/warp` lock `<name>` to lock or unlock a warp against use. |
| `uxmessentials.warp.move` | `op` | `/movewarp <name>` to move an existing warp to your current location. |
| `uxmessentials.warp.others` | `op` | `/warp <name> <player>` to send another player to a warp. |
| `uxmessentials.warp.password` | `op` | `/warp` password `<name>` to set or clear a warp's access password. |
| `uxmessentials.warp.set` | `op` | `/setwarp <name>` to create or move a server-wide warp. |
| `uxmessentials.warp.sign.create` | `op` | Create a `[warp]` sign that teleports players to a warp on click. |
| `uxmessentials.warp.sign.use` | `true` | Use a `[warp]` sign to teleport to its warp. |
| `uxmessentials.warp.use` | `true` | `/warp <name>` to teleport to a server warp. |
| `uxmessentials.warp.use.<warp>` | `label` | Use one warp, when that warp is configured to require a permission. |

Per-warp gates are data-driven in `warps.conf`: a warp with `permission: true` requires the family node `uxmessentials.warp.use.<warp>` (`<warp>` is the warp id, lower-cased); a warp with a `cost` charges through the economy provider.

---

## Player Warps

Player-owned warps are keyed by `(owner, name)`: two players may each keep a `base`. The base nodes are self-service: holding one lets a player *run* the verb, while whether they may run it on a **particular** warp is decided by the owner/co-owner/manager role matrix inside the module. See [Player Warps](../features/player-warps.md).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.pwarp.ban` | `true` | `/pwarp` ban\|unban `<name> <player>` to bar a player from one of your warps or lift that bar. |
| `uxmessentials.pwarp.bypass.ban` | `op` | Enter a player warp you are banned from (skips the ban check on `/pwarp`). |
| `uxmessentials.pwarp.bypass.password` | `op` | Enter a password-protected player warp without the password (skips the check on `/pwarp`). |
| `uxmessentials.pwarp.bypass.whitelist` | `op` | Enter a whitelist-only player warp without being on the whitelist (skips the check on `/pwarp`). |
| `uxmessentials.pwarp.category` | `true` | `/pwarp` category `<name> [categoryId]` to file a player warp under a browse category. |
| `uxmessentials.pwarp.delete` | `true` | `/pwarp` del `<name>` to remove one of your player warps. |
| `uxmessentials.pwarp.description` | `true` | `/pwarp` description `<name> [text]` to set or clear a player warp's description. |
| `uxmessentials.pwarp.displayname` | `true` | `/pwarp` displayname `<name> [text]` to set or clear a player warp's display name. |
| `uxmessentials.pwarp.icon` | `true` | `/pwarp` icon `<name> [icon]` to set or clear a player warp's browse icon. |
| `uxmessentials.pwarp.limit.<n>` | `quota` | How many player warps you may own; the largest tier held wins. |
| `uxmessentials.pwarp.members` | `true` | `/pwarp` members `<name>` add\|remove `<player>` to grant or revoke a co-owner or manager on one of your warps. |
| `uxmessentials.pwarp.move` | `true` | `/pwarp` move `<name>` to re-anchor one of your player warps at your location. |
| `uxmessentials.pwarp.password` | `true` | `/pwarp` password `<name> <password>`\|clear to set or clear a player warp's password. |
| `uxmessentials.pwarp.price` | `true` | `/pwarp` price `<name> <amount> [currency]` to set a player warp's entry price. |
| `uxmessentials.pwarp.public` | `true` | `/pwarp` visibility public\|private `<name>` to toggle a player warp's visibility. |
| `uxmessentials.pwarp.rename` | `true` | `/pwarp` rename `<name> <newName>` to rename one of your player warps. |
| `uxmessentials.pwarp.set` | `true` | `/setpwarp <name>` to create or move a player-owned warp at your location. |
| `uxmessentials.pwarp.sponsor` | `true` | `/pwarp` sponsor `<name> [days]` to buy a paid, time-limited pinned browse slot for one of your warps. |
| `uxmessentials.pwarp.transfer` | `true` | `/pwarp` transfer `<name> <player>` to hand ownership of one of your warps to another player. |
| `uxmessentials.pwarp.whitelist` | `true` | `/pwarp` whitelist `<name>` add\|remove `<player>` to manage a whitelist-access warp's guest list. |
| `uxmessentials.pwarp.withdraw` | `true` | `/pwarp` withdraw `<name>` to pay one of your warps' accrued earnings out to its owner. |
| `uxmessentials.module.playerwarps` | `op` | Hot-reload / inspect the playerwarps module (player-owned warps behind `/pwarp`). |
| `uxmessentials.pwarp.access` | `true` | `/pwarp` access `<name> <PUBLIC\|PASSWORD\|WHITELIST\|PRIVATE>` to set a player warp's access axis. |
| `uxmessentials.pwarp.admin` | `op` | `/pwarp` admin restore\|purge\|setowner\|reload to manage any player's warp by its id. |
| `uxmessentials.pwarp.bypass.cost` | `op` | Use a priced player warp without paying its entry cost (skips the charge on `/pwarp`). |
| `uxmessentials.pwarp.bypass.safety` | `op` | Use a player warp whose destination is unsafe (skips the safe-landing check on `/pwarp`). |
| `uxmessentials.pwarp.favourite` | `true` | `/pwarp` favourite\|unfavourite `<name>` to star or un-star a player warp. |
| `uxmessentials.pwarp.edit` | `true` | `/pwarp` edit `<name>` to open one warp's property editor, the click-driven form of the typed verbs. Held by default alongside the use node. |
| `uxmessentials.pwarp.gui` | `op` | Manage every player's warps in the `/pwarp` GUI (a player without it edits only their own). |
| `uxmessentials.pwarp.info` | `true` | `/pwarp` info `<name>` to show a player warp's owner, access, price, visits, and rating. |
| `uxmessentials.pwarp.list` | `true` | `/pwarps [player]` to list your warps or a player's public warps. |
| `uxmessentials.pwarp.rate` | `true` | `/pwarp` rate `<name> <1-5>` to award a player warp a star rating. |
| `uxmessentials.pwarp.use` | `true` | `/pwarp <name> [owner]` to teleport to your own or a player's public warp. |

The five bypasses (`uxmessentials.pwarp.bypass.ban`, `.password`, `.whitelist`, `.safety`, `.cost`) each default `op` and are listed under [Bypass Nodes](#bypass-nodes). The per-player cap is the tier `uxmessentials.pwarp.limit.<n>`, optionally scoped per world.

---

## Economy

Balances are DB-backed and routed through the shared `EconomyProvider` port; eco-admin mutations are always audit-logged.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.economy.admin` | `op` | Umbrella for eco-admin mutations (`/eco` give, take, set). |
| `uxmessentials.economy.admin.bulk` | `op` | `/eco` giveall, `/eco` giverandom, `/eco` resetall server-wide bulk mutations. |
| `uxmessentials.economy.admin.give` | `op` | `/eco` give `<player> <amount> [currency]` only. |
| `uxmessentials.economy.admin.set` | `op` | `/eco` set `<player> <amount> [currency]` and `/eco` reset only. |
| `uxmessentials.economy.admin.take` | `op` | `/eco` take `<player> <amount> [currency]` only. |
| `uxmessentials.economy.balance` | `true` | `/balance [currency]` to see your own balance. |
| `uxmessentials.economy.balance.others` | `op` | `/balance <player> [currency]` to view another player's balance. |
| `uxmessentials.economy.baltop` | `true` | `/baltop [currency] [page]` to view the top balances. |
| `uxmessentials.economy.baltop.exempt` | `false` | Marks the holder as hidden from every `/baltop` leaderboard. |
| `uxmessentials.economy.bank` | `true` | `/bank`: open the bank panel to move money between your wallet and your bank balance. |
| `uxmessentials.economy.bank.create` | `true` | `/bank` create `<name>`: open a new shared bank account. Held by default alongside the bank node. |
| `uxmessentials.economy.bank.deposit` | `true` | `/bank` deposit: pay into a shared bank account. Held by default alongside the bank node. |
| `uxmessentials.economy.bank.withdraw` | `true` | `/bank` withdraw: take money out of a shared bank account; the capability worth withholding from junior members. |
| `uxmessentials.economy.bank.members` | `true` | `/bank` addmember and removemember: change who may use a shared bank account. |
| `uxmessentials.economy.bypasscmdcost` | `op` | Skip the configured per-command economy charge (command-costs in economy.conf). |
| `uxmessentials.economy.currency.<currency>` | `label` | Use one currency that is configured to require a permission. |
| `uxmessentials.economy.deposit` | `true` | `/deposit <amount> [currency]`: move money from your wallet into your bank balance. |
| `uxmessentials.economy.exchange` | `true` | `/exchange <amount> <from> <to>`: convert between two currencies at the configured rate. |
| `uxmessentials.economy.loan` | `false` | `/loan`: take, review and repay a loan against the configured limit and interest. |
| `uxmessentials.economy.loan.take` | `true` | `/loan` take: borrow. Held by default alongside the loan node, so negating it leaves a borrower able to check and repay what they already owe but not to take more. |
| `uxmessentials.economy.pay` | `true` | `/pay <player> <amount> [currency]` and `/payconfirm` to transfer funds. |
| `uxmessentials.economy.pay.toggle` | `true` | `/paytoggle` to refuse all incoming `/pay` transfers. |
| `uxmessentials.economy.payall` | `op` | `/payall <amount> [currency]`: pay every online player from your own wallet. |
| `uxmessentials.economy.salary.amount.<amount>` | `quota` | The periodic salary you are paid; the largest tier held wins. |
| `uxmessentials.economy.sell` | `true` | `/sell [amount]` to sell held items at their configured worth. |
| `uxmessentials.economy.setworth` | `op` | `/setworth [item] <price>`\|clear to set or clear an item's sell worth override. |
| `uxmessentials.economy.tax.bypass` | `op` | Send a `/pay` without the configured transfer tax being deducted. |
| `uxmessentials.economy.wallet` | `true` | `/wallet`: open your own wallet panel listing every currency you hold. |
| `uxmessentials.economy.withdraw` | `true` | `/withdraw <amount> [currency]`: move money from your bank balance back into your wallet. |
| `uxmessentials.economy.worth` | `true` | `/worth [item]` to report an item's configured sell value. |
| `uxmessentials.module.economy` | `op` | Hot-reload / inspect the economy module (wallets, banks, currencies and the provider bridge). |

`/bank`, `/deposit`, `/withdraw`, `/loan`, `/wallet`, `/exchange`, `/sellall`, and the extra `/eco` subcommands carry their own nodes but are only active when the matching feature flag is on in `economy.conf`. Per-currency gating uses the tier node `uxmessentials.economy.currency.<id>`: see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Ranks

Rankup, prestige and the ladder panel. The plugin tracks each player's rank with a DB-backed pointer of its own; a rank's rankup actions may set a permission-plugin group, run any command, or nothing at all. The ladder (ranks, order, cost, requirements, actions) is data-driven in `modules/ranks/ranks.conf`.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.ranks` | `op` | Hot-reload / inspect the ranks module (rank ladders, prestige and rank-up costs). |
| `uxmessentials.ranks.admin` | `op` | `/ranks` setrank `<player> <rank>` to set a player's rank directly. |
| `uxmessentials.ranks.gui` | `true` | `/ranks` to open the ladder panel (config-gated; registered only when the GUI is enabled). |
| `uxmessentials.ranks.prestige` | `true` | `/prestige` to reset to the first rank for a prestige level once you reach the top rank. |
| `uxmessentials.ranks.rankup` | `true` | `/rankup` to advance to the next rank when you meet its requirements. |

A rank's `cost` is charged through the economy provider, and a `placeholder` requirement uses an operator-chosen `%placeholder%` comparison: neither is a plugin-declared node.

---

## Kits

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.kit.<kit>` | `label` | Claim one kit. |
| `uxmessentials.kit.cooldown.<kit>.<seconds>` | `tier` | The wait between claims of one kit, in seconds; the shortest tier held wins. |
| `uxmessentials.kit.cooldown.bypass` | `op` | Skip kit cooldowns and re-claim one-time kits. |
| `uxmessentials.kit.edit` | `op` | `/kit` create, `/kit` del, `/kit` editor to define, remove and edit kit contents. |
| `uxmessentials.kit.others` | `op` | `/kit <name> <player>` to give a kit to another player. |
| `uxmessentials.kit.preview` | `true` | `/kit` show `<name>` to preview a kit's contents without claiming it. |
| `uxmessentials.kit.reset` | `op` | `/kit` reset `<player> [kit]` to clear a player's claim/cooldown stamps. |
| `uxmessentials.kit.use` | `true` | `/kit <name>` to claim a kit and `/kit` list to list the kits you may claim. |
| `uxmessentials.module.kits` | `op` | Hot-reload / inspect the kits module (kit definitions, cooldowns and claims). |
| `uxmessentials.oversizedstacks` | `op` | Receive kit items in stacks larger than the material normally allows. |

Per-kit gating is data-driven: a kit with `permission: true` requires `uxmessentials.kit.<id>`.

---

## Vaults

DB-persisted, player-owned item storage. Vault count and per-vault size are numbered quota tiers.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.vaults` | `op` | Hot-reload / inspect the vaults module (DB-persisted player vault storage). |
| `uxmessentials.vault.admin.delete` | `op` | `/vault` delete `<player> <n>`: delete another player's vault (audit-logged, no refund). |
| `uxmessentials.vault.amount.<n>` | `quota` | How many vaults you may open; the largest tier held wins. |
| `uxmessentials.vault.bypass-blacklist` | `op` | Store any item in a vault, ignoring the blacklist-materials list (items are not returned). |
| `uxmessentials.vault.free` | `false` | Bypass every vault economy fee (create/open); no refund is paid on delete. |
| `uxmessentials.vault.icon` | `true` | `/vault` icon `<n> [material]`: set or clear the icon of your own vault (held item if omitted). |
| `uxmessentials.vault.others` | `op` | `/vault <player> [n]`: open and audit another player's vault (audit-logged). |
| `uxmessentials.vault.rename` | `true` | `/vault` rename `<n> [name]`: set or clear the display name of your own vault. |
| `uxmessentials.vault.size.<rows>` | `quota` | How many rows each of your vaults holds; the largest tier held wins. |
| `uxmessentials.vault.use` | `true` | `/vault` to open your default vault (or list them), `/vault <n>` to open the Nth, and `/vault` delete `<n>` to delete your own. |

---

## Trade

Player-to-player trading through a shared, both-confirm window. `/trade` is the whole surface; the accept/deny answers and the (optional) cross-server escrow all ride on the one node.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.trade` | `op` | Hot-reload / inspect the trade module. |
| `uxmessentials.trade.use` | `true` | `/trade`: request a trade with another player and accept or deny requests. |

The item blacklist, request distance, cooldown, staked currencies, audit, and cross-server escrow are config-driven in `modules/trade/config.conf`, not permission nodes.

---

## Player State

Toggleable flags and apply-once effects. Every `[player]` target form additionally requires `uxmessentials.playerstate.others`. These default to `op` because they are powerful self/other buffs: loosen the specific ones a donor rank should get.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.air.use` | `op` | `/air <seconds>` to set a player's remaining air. |
| `uxmessentials.burn.use` | `op` | `/burn <seconds>` to set a player on fire. |
| `uxmessentials.exp.use` | `op` | `/exp` (`/xp`) get\|set\|give\|take\|reset to read or change experience. |
| `uxmessentials.extinguish.use` | `op` | `/ext` (`/extinguish`) `[player]` to put out a burning player. |
| `uxmessentials.feed.use` | `op` | `/feed [player]` to restore hunger. |
| `uxmessentials.fly.use` | `op` | `/fly [player]` to toggle flight. |
| `uxmessentials.foodlevel.use` | `op` | `/foodlevel <amount> [player]` to set a player's food level. |
| `uxmessentials.gamemode.use` | `op` | `/gamemode <mode> [player]` and the `/gmc /gms /gma /gmsp` aliases. |
| `uxmessentials.glow.use` | `op` | `/glow` to toggle a glowing outline on yourself. |
| `uxmessentials.god.use` | `op` | `/god [player]` to toggle damage immunity. |
| `uxmessentials.heal.use` | `op` | `/heal [player]` to restore health. |
| `uxmessentials.health.use` | `op` | `/health <amount> [player]` to set a player's health. |
| `uxmessentials.ice.use` | `op` | `/ice [player] [seconds]` to freeze a player (inverse of `/burn`). |
| `uxmessentials.nightvision.use` | `op` | `/nightvision` (`/nv`) to toggle a night-vision effect on yourself. |
| `uxmessentials.playerstate.fly.allworlds` | `op` | Keep flight in worlds where flying is switched off. |
| `uxmessentials.ptime.use` | `op` | `/ptime <value\|reset>` to set a per-player client-side time. |
| `uxmessentials.pweather.use` | `op` | `/pweather <clear\|rain\|reset>` to set a per-player client-side weather. |
| `uxmessentials.rest.use` | `op` | `/rest` to reset a player's time-since-rest so phantoms stop. |
| `uxmessentials.speed.use` | `op` | `/speed`, `/walkspeed` and `/flyspeed` to set walk/fly speed. |
| `uxmessentials.suicide.use` | `true` | `/suicide` to kill yourself. |
| `uxmessentials.biome.use` | `true` | `/biome` to show the biome you are standing in. |
| `uxmessentials.depth.use` | `true` | `/depth` to show your height relative to sea level. |
| `uxmessentials.dimension.use` | `true` | `/dimension` to show the dimension you are standing in. |
| `uxmessentials.endersee.use` | `op` | `/endersee [player]` to view a player's ender chest. |
| `uxmessentials.getpos.use` | `op` | `/getpos` (`/coords /whereami`) to show a player's coordinates. |
| `uxmessentials.invsee.modify` | `op` | Edit a player's inventory through the `/invsee` menu (without this it is view-only). |
| `uxmessentials.invsee.use` | `op` | `/invsee [player]` to view a player's inventory. |
| `uxmessentials.near.use` | `op` | `/near [radius]` to list nearby players. |
| `uxmessentials.ping.use` | `true` | `/ping` to show a player's round-trip latency. |
| `uxmessentials.playtime.reset` | `op` | `/playtime` reset `[player]` to wipe a player's tracked playtime (resetting others also needs uxmessentials.playerstate.others). |
| `uxmessentials.playtime.use` | `op` | `/playtime [player]` to show a player's playtime breakdown (active/afk, today/week/month/all-time). |
| `uxmessentials.seed.use` | `true` | `/seed` to show the seed of the world you are standing in. |
| `uxmessentials.world.command-bypass` | `op` | Run commands a world blocks through its per-world command list. |
| `uxmessentials.world.use` | `true` | `/world` to show the world you are standing in. |
| `uxmessentials.clearinventory.confirmtoggle` | `true` | `/clearinventoryconfirmtoggle` (`/citoggle`) to require a confirmation before `/clearinventory` clears your own inventory. |
| `uxmessentials.clearinventory.use` | `op` | `/clearinventory` (`/ci /clear`) `[player]` to empty a player's inventory. |
| `uxmessentials.compass.use` | `true` | `/compass` to show the direction you are facing. |
| `uxmessentials.module.playerstate` | `op` | Hot-reload / inspect the playerstate module (flight, god mode, speed, health and the rest). |
| `uxmessentials.air.others` | `op` | `/air` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.burn.others` | `op` | `/burn` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.clearinventory.others` | `op` | `/clearinventory` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.endersee.others` | `op` | `/endersee` to open another player ender chest. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.exp.others` | `op` | `/exp` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.extinguish.others` | `op` | `/extinguish` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.feed.others` | `op` | `/feed` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.fly.others` | `op` | `/fly` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.foodlevel.others` | `op` | `/foodlevel` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.gamemode.others` | `op` | `/gamemode` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.getpos.others` | `op` | `/getpos` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.god.others` | `op` | `/god` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.heal.others` | `op` | `/heal` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.health.others` | `op` | `/health` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.ice.others` | `op` | `/ice` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.invsee.others` | `op` | `/invsee` to open another player inventory. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.ping.others` | `op` | `/ping` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.playtime.others` | `op` | `/playtime` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.rest.others` | `op` | `/rest` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.speed.others` | `op` | `/speed` on another player. Granted on its own, or by the cross-cutting uxmessentials.playerstate.others. |
| `uxmessentials.playerstate.others` | `op` | Use any playerstate command with a `[player]` target other than yourself. |

---

## Messaging

Private messages and persistent mail only (not public chat). Delivery is ignore-aware and respects a `moderation` mute.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.helpop.receive` | `op` | Receive `/helpop` requests (staff side). |
| `uxmessentials.helpop.use` | `true` | `/helpop <text>` to open a player-to-staff support request. |
| `uxmessentials.mail.sendall` | `op` | `/mail` sendall `<text>` to broadcast mail to every online player (staff). |
| `uxmessentials.mail.use` | `true` | `/mail` read / `/mail` send `<player> <text>` / `/mail` clear (`/mailclear`): persistent offline mail. |
| `uxmessentials.messaging.gui` | `op` | See and open the messaging settings panel and mailbox on the `/uxmess` gui management hub. |
| `uxmessentials.module.messaging` | `op` | Hot-reload / inspect the messaging module (private messages, mail and social spy). |
| `uxmessentials.msg.color` | `op` | Render MiniMessage tags in PM/mail bodies (default plain text). |
| `uxmessentials.msg.ignore` | `true` | `/ignore <player>` / `/unignore <player>` / `/ignorelist` to manage and view your own ignore list. |
| `uxmessentials.msg.reply` | `true` | `/reply <text>` to answer your last conversation (reply-TTL bounded). |
| `uxmessentials.msg.socialspy` | `op` | `/socialspy` to observe other players' private messages (staff). |
| `uxmessentials.msg.toggle` | `true` | `/msgtoggle` to refuse incoming `/msg` / `/reply`; `/rtoggle` to refuse only incoming `/reply` routing (mail still delivers). |
| `uxmessentials.msg.use` | `true` | `/msg <player> <text>` to send a private message. |
| `uxmessentials.msgsettings.use` | `true` | `/msgsettings` opens your personal messaging settings panel (accept-messages, social spy). |

---

## Communication

Connection-message policies, the rotating announcer, and info pages. Operator-authored bodies are config content; the nodes gate the *commands*.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.announce.admin` | `op` | `/announce` reload\|list\|preview\|toggle to manage the rotating announcer. |
| `uxmessentials.announce.admin.editor` | `true` | `/announce` editor to author announcements in the GUI. |
| `uxmessentials.announce.admin.reload` | `true` | `/announce` reload to re-read announcer.conf and re-arm the rotation. |
| `uxmessentials.announce.admin.list` | `true` | `/announce` list to read the rotation and the channels each announcement pushes to. |
| `uxmessentials.announce.admin.preview` | `true` | `/announce` preview `<id>` to show one announcement to yourself alone. |
| `uxmessentials.announce.admin.toggle` | `true` | `/announce` toggle to stop or resume receiving the rotating announcements. |
| `uxmessentials.communication.broadcast` | `op` | `/broadcast` to send a one-off announcement to all online players. |
| `uxmessentials.communication.broadcasttoggle` | `true` | `/broadcasttoggle` to stop or resume receiving the rotating server announcements. |
| `uxmessentials.communication.broadcastworld` | `op` | `/broadcastworld` (alias `/bcw`) to send a one-off announcement only to players in your world. |
| `uxmessentials.communication.chat.bypass` | `op` | Keep chatting while public chat is locked by `/togglechat`. |
| `uxmessentials.communication.chat.format` | `false` | Use MiniMessage formatting in your own public chat messages (when allow-player-format is on). |
| `uxmessentials.communication.clearchat` | `op` | `/clearchat` (alias `/chatclear`) to flush the chat for online players. |
| `uxmessentials.communication.clearchat.exempt` | `false` | Keep your chat scrollback when staff run `/clearchat`. |
| `uxmessentials.communication.gui` | `op` | Open the communication admin panel (`/communication` gui and on the `/uxmess` gui hub). |
| `uxmessentials.communication.info.<page>` | `label` | Read one information page, such as `/motd` or `/rules`. |
| `uxmessentials.communication.info.info` | `true` | `/info` to read the shipped welcome / quick-start info page. |
| `uxmessentials.communication.info.motd` | `true` | `/motd` to read the shipped message-of-the-day info page. |
| `uxmessentials.communication.info.rules` | `true` | `/rules` to read the shipped server-rules info page. |
| `uxmessentials.communication.me` | `true` | `/me` to broadcast a third-person action message to all online players. |
| `uxmessentials.communication.togglechat` | `op` | `/togglechat` (alias `/mutechat`) to lock or unlock public chat for non-staff. |
| `uxmessentials.module.communication` | `op` | Hot-reload / inspect the communication module (connection messages, announcer, info pages). |

---

## Moderation

Every action is audit-logged and permission-gated. A mute blocks outbound messaging; a jail blocks `/home` and `/tpa`.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.moderation.<sanction>.maxduration.<seconds>` | `tier` | The longest ban, mute or jail you may hand out, in seconds; the largest tier held wins. |
| `uxmessentials.moderation.ban` | `op` | `/ban <player> [reason]` and `/unban <player>`: permanent UUID ban and its lift; `/banhistory <player>` reviews a player's full ban/unban history. |
| `uxmessentials.moderation.banip` | `op` | `/banip <player\|ip> [reason]` / `/unbanip <ip>`: IP ban with stored-IP alt detection. |
| `uxmessentials.moderation.banlist` | `op` | `/banlist` to review the players currently banned. |
| `uxmessentials.moderation.freeze` | `op` | `/freeze <player>` / `/unfreeze <player>`: pin a player in place pending review. |
| `uxmessentials.moderation.jail` | `op` | `/jail <player> <jail> [duration] [reason]`; `/jails` lists configured jails; `/jailedplayers` lists who is jailed; `/setjail <name>` defines a jail at your location; `/jail` del `<name>` removes a defined jail. |
| `uxmessentials.moderation.kick` | `op` | `/kick <player> [reason]`; `/kickall [reason]` to clear non-exempt players. |
| `uxmessentials.moderation.lockdown` | `op` | `/lockdown [on\|off]`: refuse every login except holders of the lockdown bypass; the flag survives restart. |
| `uxmessentials.moderation.lockdown.bypass` | `op` | Join the server while it is locked down (`/lockdown`). Held by staff who must stay reachable during a lockdown. |
| `uxmessentials.moderation.mute` | `op` | `/mute <player> [duration] [reason]` (`/tempmute` is the explicit duration alias); `/mutehistory <player>` reviews a player's full mute/unmute history. |
| `uxmessentials.moderation.mutelist` | `op` | `/mutelist` to review the players currently muted. |
| `uxmessentials.moderation.sanction` | `op` | `/sanction <player>`: aggregated read-only punishment summary: current mute, jail, ban state and active warning count. |
| `uxmessentials.moderation.sudo` | `op` | `/sudo <player> <command>`: run a command as another player. |
| `uxmessentials.moderation.tempban` | `op` | `/tempban <player> <duration> [reason]`. |
| `uxmessentials.moderation.templates` | `op` | `/punish <player> <template>`: apply a configured punishment template (a preset reason + optional duration) as a ban or tempban. |
| `uxmessentials.moderation.togglejail` | `op` | `/togglejail <player> [jail] [reason]`: release the target if jailed, otherwise jail them in the named jail (or the first configured jail). |
| `uxmessentials.moderation.unjail` | `op` | `/unjail <player>`. |
| `uxmessentials.moderation.unmute` | `op` | `/unmute <player>`. |
| `uxmessentials.moderation.warn` | `op` | `/warn <player> [reason]`, `/tempwarn <player> <duration> [reason]`, `/warns <player>` and `/unwarn <player>`: issue (standing or timed), review and clear warning history. |
| `uxmessentials.moderation.broadcast.receive` | `op` | Receive the staff sanction broadcast: the one-line announcement a non-silent `/ban /mute /kick /warn` emits. The -s flag (or broadcast.silent-by-default) suppresses it. Duration tiers ride numbered nodes: uxmessentials.moderation.ban.maxduration.`<seconds>` and uxmessentials.moderation.mute.maxduration.`<seconds>` cap how long a ban/mute that holder may issue (highest held wins; no node = unlimited). |
| `uxmessentials.moderation.check` | `op` | `/checkban <player>` and `/checkmute <player>`: report whether a player is currently banned or muted. |
| `uxmessentials.moderation.commandspy` | `op` | `/commandspy` (`/cspy`) to watch the commands other players run. |
| `uxmessentials.moderation.exempt` | `op` | Cannot be muted/jailed/tempbanned/kicked/warned/IP-banned/frozen by lower staff. |
| `uxmessentials.moderation.gui` | `op` | `/mod` opens the moderation management GUI (active punishments + per-player history). |
| `uxmessentials.moderation.history` | `op` | `/history <player>`: review a player's full disciplinary record (ban/mute/warn/kick) newest-first. |
| `uxmessentials.moderation.seen` | `op` | `/seen`, `/seenip` and `/alts <player>`: last-seen / last-IP lookup, surfaces alts. |
| `uxmessentials.moderation.staffhistory` | `op` | `/staffhistory <staff>`: review the sanctions a staff member has issued, newest-first. |
| `uxmessentials.moderation.staffrollback` | `op` | `/staffrollback <staff> [limit]`: revoke a staff member's still-active sanctions (un-ban/un-mute/clear-warns the targets they sanctioned). |
| `uxmessentials.moderation.stats` | `op` | `/modstats [staff] [days]`: staff punishment analytics: a most-active-staff leaderboard or a single staff member's breakdown, optionally over the last N days. |
| `uxmessentials.module.moderation` | `op` | Hot-reload / inspect the moderation module (bans, mutes, jails, warnings and history). |

Ban and mute durations can be capped per rank with the numbered families `uxmessentials.moderation.ban.maxduration.<seconds>` and `.mute.maxduration.<seconds>`: see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Staff

STAFF-MODE ONLY: a loadout swap and a gadget hotbar. It issues no sanctions of its own; the FREEZE and COMPASS gadgets orchestrate the moderation and teleport use cases. The `/staff` text roster and the `uxmessentials.staff.member` marker live under [Presence](#presence).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.staff` | `op` | Hot-reload / inspect the staff module (staff mode, its loadout and staff chat). |
| `uxmessentials.staff.chat` | `op` | `/staffchat` (alias `/sc`) to send and receive lines on the staff-only chat channel. |
| `uxmessentials.staff.list` | `op` | `/stafflist` to open the online-staff GUI (vanish-aware) and click a head to teleport to that staff member. |
| `uxmessentials.staff.mode` | `op` | `/staffmode [player]` to flip into staff mode: your real loadout is saved and swapped for the gadget hotbar (and you vanish); leaving restores it. The EXAMINE gadget opens a player's inventory. |

---

## Vanish

PremiumVanish-class invisibility with layered see/use levels. Its own `vanish` module: the single authority every other context reads for vanish state. See the [Vanish](../features/vanish.md) guide for the level rules.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.vanish` | `op` | Hot-reload / inspect the vanish module. |
| `uxmessentials.vanish.list` | `op` | `/vanish` list to see the hidden players you are permitted to see (scoped to your see level). |
| `uxmessentials.vanish.others` | `op` | `/vanish <player>` to toggle another player's vanish. |
| `uxmessentials.vanish.persist` | `op` | Remain vanished across a relog instead of reappearing on join. |
| `uxmessentials.vanish.see` | `op` | See other vanished players (staff-among-staff visibility) and target them with `/tp`. |
| `uxmessentials.vanish.see.level<n>` | `quota` | How deeply you see: a viewer sees a vanished player when their see level reaches that player's use level; the largest level held wins and plain .see is level 1. |
| `uxmessentials.vanish.use.level<n>` | `quota` | How deeply you vanish: only a viewer whose see level reaches this level finds you; the largest level held wins and plain .use is level 1. |
| `uxmessentials.vanish.silent` | `op` | `/vanish` -s to vanish or reappear without the fake join/quit broadcast. |
| `uxmessentials.vanish.use` | `op` | `/vanish` to become invisible to other players; suppresses fake join/quit. |

The optional layered families `uxmessentials.vanish.use.level<N>` (vanish at use level `N`) and `uxmessentials.vanish.see.level<N>` (see up to level `N`) tier the see/use model: a viewer sees a vanished player only when their see level is at least the target's use level. Plain `.use` / `.see` are level 1; see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes).

---

## Presence

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.afk.use` | `true` | `/afk [reason]` to toggle your AFK state (auto-AFK also applies on idle). |
| `uxmessentials.gc.use` | `op` | `/gc` to show server health: TPS, uptime, memory and loaded chunks. |
| `uxmessentials.list.use` | `true` | `/list` to see who is online. |
| `uxmessentials.module.presence` | `op` | Hot-reload / inspect the presence module (AFK, nicknames and vanish state). |
| `uxmessentials.nick.others` | `op` | `/nick <player> <name>` to set another player's display name. |
| `uxmessentials.nick.use` | `true` | `/nick <name>` \| off to set or clear your display name. |
| `uxmessentials.presence.gui` | `op` | Show the presence settings panel on the `/uxmess` gui hub. |
| `uxmessentials.presencesettings.use` | `true` | `/presencesettings` opens your personal presence settings panel. |
| `uxmessentials.realname.use` | `true` | `/realname <player>` to look up a player's real account name. |
| `uxmessentials.staff.member` | `op` | Marks a player as staff so they appear in `/staff`. |
| `uxmessentials.staff.use` | `op` | `/staff` to list online staff members. |
| `uxmessentials.whois.use` | `op` | `/whois <player>` to view an online player's account, identity and status. |

---

## Worlds

Multi-world management. Nodes use the `uxmessentials.world.*` prefix (the plural `/worlds` command; `playerstate`'s singular `/world` info verb also reads `uxmessentials.world.use`).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.worlds` | `op` | Hot-reload / inspect the worlds module (world creation, properties and access). |
| `uxmessentials.world.<world>` | `label` | Enter one world that is configured as restricted. |
| `uxmessentials.world.access.bypass` | `op` | Enter a restricted world without holding that world's own entry node. |
| `uxmessentials.world.backup` | `op` | `/world` backup `<name>`: snapshot a world's folder. |
| `uxmessentials.world.create` | `op` | `/world` create `<name>`: generate and register a new world. |
| `uxmessentials.world.delete` | `op` | `/world` delete `<name>`: unregister a world and delete its folder from disk. |
| `uxmessentials.world.gamemode.bypass` | `op` | Keep your own game mode in a world that forces one on entry. |
| `uxmessentials.world.gamerule` | `op` | `/world` gamerule `<name> <rule> <value>`: change one gamerule on a world. |
| `uxmessentials.world.gui` | `op` | `/world`: open the world management GUI. |
| `uxmessentials.world.import` | `op` | `/world` import `<folder>`: adopt an existing world folder into the registry. |
| `uxmessentials.world.info` | `op` | `/world` info `<name>`: read one world's generator, properties, gamerules and spawn. |
| `uxmessentials.world.list` | `op` | `/world` list: list every registered world with its load state and player count. |
| `uxmessentials.world.load` | `op` | `/world` load `<name>`: load a registered world that is currently unloaded. |
| `uxmessentials.world.pregen` | `op` | `/world` pregen `<name> <radius>`: pre-generate a world's chunks in the background. |
| `uxmessentials.world.restore` | `op` | `/world` restore `<name> <backup>`: restore a world from one of its snapshots. |
| `uxmessentials.world.set` | `op` | `/world` set `<name> <property> <value>`: change a world property such as difficulty or PvP. |
| `uxmessentials.world.setspawn` | `op` | `/world` setspawn `[name]`: set a world's spawn point to where you are standing. |
| `uxmessentials.world.spawn` | `true` | `/worlds` spawn to teleport to a world's spawn (subject to per-world access rules). |
| `uxmessentials.world.tp` | `op` | `/world` tp `<name>`: teleport yourself to a world's spawn. |
| `uxmessentials.world.tp.others` | `op` | `/world` tp `<name> <player>`: teleport somebody else to a world's spawn. |
| `uxmessentials.world.unload` | `op` | `/world` unload `<name>`: unload a loaded world, moving anybody inside to spawn. |
| `uxmessentials.world.unregister` | `op` | `/world` unregister `<name>`: drop a world from the registry, leaving its folder on disk. |

Per-world entry uses `uxmessentials.world.<name>.enter` (a world with access restrictions). Bypass nodes are listed under [Bypass Nodes](#bypass-nodes).

---

## Item & World

The item/world toolbox carries roughly 65 verbs, each with its own node; the full per-command list lives on the [Items, Blocks & World](../commands/items-world.md) command page. They default `op`, except the read-only inspection verbs (`/itemdb`, `/iteminfo`, `/recipe`) which are self-service. Two nodes are worth calling out here: the held-item editor, and the in-inventory shulker editor, which has no command of its own.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.book.use` | `op` | `/book`: unlock a written book for editing. |
| `uxmessentials.condense.use` | `op` | `/condense` (alias `/compact`) `[all]`: recipe-stack inventory items. |
| `uxmessentials.disenchant.use` | `op` | `/disenchant [all\|<enchant>]`: remove all or one enchantment from the held item. |
| `uxmessentials.editsign.use` | `op` | `/editsign`: edit the sign you are looking at (respects build access). |
| `uxmessentials.enchant.use` | `op` | `/enchant <enchant> [level]`: enchant the held item (level clamped at the boundary). |
| `uxmessentials.firework.use` | `op` | `/firework <color\|clear\|power>`: style or power the held firework rocket. |
| `uxmessentials.give.use` | `op` | `/give <player> <item> [amount]` (alias `/i`): give an item to a player; bulk gives are audited. |
| `uxmessentials.giveall.use` | `op` | `/giveall <item> [amount]`: give an item to every online player; bulk gives are audited per recipient. |
| `uxmessentials.hat.use` | `op` | `/hat`: wear the held item as a helmet (itemworld-owned; playerstate defers it). |
| `uxmessentials.item.use` | `op` | `/item <item> [amount]`: give an item to yourself. |
| `uxmessentials.itemamount.use` | `op` | `/itemamount <amount>` (`/amount`): set the held stack amount, clamped to the give cap. |
| `uxmessentials.itemdamage.use` | `op` | `/itemdamage <damage>` (`/durability`): set the held item's durability damage. |
| `uxmessentials.itemdb.use` | `op` | `/itemdb [item]`: look up an item's id / data. |
| `uxmessentials.itemflag.use` | `op` | `/itemflag <flag> <on\|off>`: toggle an item meta flag. |
| `uxmessentials.iteminfo.use` | `op` | `/iteminfo`: inspect the metadata of the item in your hand. |
| `uxmessentials.itemlore.use` | `op` | `/itemlore <set\|add\|clear> [text]`: edit the held item's lore. |
| `uxmessentials.itemmodel.use` | `op` | `/itemmodel <id\|clear>` (alias `/custommodeldata`): set or clear the held item's custom model data. |
| `uxmessentials.itemname.use` | `op` | `/itemname <name>`: rename the held item. |
| `uxmessentials.itemworld.enchant.<enchantment>` | `label` | Apply one specific enchantment through `/enchant`, when per-enchantment gating is switched on. |
| `uxmessentials.itemworld.give.<item>` | `label` | Give one specific item through `/give`, when per-item gating is switched on. |
| `uxmessentials.itemworld.gui` | `op` | Open the itemworld utilities hub (`/itemworld` gui and on the `/uxmess` gui hub). |
| `uxmessentials.itemworld.itemedit` | `op` | `/itemedit <rename\|resetname\|lore\|enchant\|unenchant\|flag\|attribute\|durability\|repair\|unbreakable\|custommodeldata>`: edit the held item's name, lore and meta (item-edit.enabled). |
| `uxmessentials.itemworld.itemedit.rename` | `true` | `/itemedit` rename and resetname. Defaults on for anyone who already holds the editor node; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.lore` | `true` | `/itemedit` lore add\|set\|insert\|remove\|clear. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.enchant` | `true` | `/itemedit` enchant and unenchant. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.flag` | `true` | `/itemedit` flag: toggle an item flag such as HIDE_ENCHANTS. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.itemedit.attribute` | `true` | `/itemedit` attribute add\|remove: edit attribute modifiers. Defaults on for an editor-node holder; the verb most worth withholding on a survival server. |
| `uxmessentials.itemworld.itemedit.durability` | `true` | `/itemedit` durability and repair. Defaults on for an editor-node holder; negate it to withhold just this verb. |
| `uxmessentials.itemworld.itemedit.unbreakable` | `true` | `/itemedit` unbreakable: mark the held item unbreakable. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.itemedit.model` | `true` | `/itemedit` custommodeldata: set or clear the resource-pack model selector. Defaults on for an editor-node holder. |
| `uxmessentials.itemworld.itemedit.gui` | `true` | A bare `/itemedit`, which opens the click-driven editor. Defaults on for an editor-node holder; negate it to leave only the typed verbs. |
| `uxmessentials.itemworld.shulker` | `op` | Right-click a shulker box in the inventory to open its contents in place (shulkers.enabled). |
| `uxmessentials.itemworld.spawnmob.<mob>` | `label` | Spawn one specific mob through `/spawnmob`, when per-mob gating is switched on. |
| `uxmessentials.module.itemworld` | `op` | Hot-reload / inspect the itemworld module and its sub-feature groups. |
| `uxmessentials.more.use` | `op` | `/more`: fill the held stack to max (itemworld-owned; playerstate defers it). |
| `uxmessentials.potion.use` | `op` | `/potion <effect> [duration] [amplifier]`: add a potion effect to the held potion. |
| `uxmessentials.recipe.use` | `op` | `/recipe [item]`: show an item's crafting recipe. |
| `uxmessentials.repair.itemworld` | `op` | `/repair` and `/repairall` in the itemworld surface (itemworld-owned; playerstate defers them). |
| `uxmessentials.showitem.use` | `true` | `/showitem`: broadcast the held item to chat for everyone online. |
| `uxmessentials.skull.use` | `op` | `/skull [player]`: get a player-head skull. |
| `uxmessentials.unbreakable.use` | `op` | `/unbreakable [true\|false]`: toggle or set the held item's unbreakable flag. |
| `uxmessentials.unlimited.use` | `op` | `/unlimited`: toggle unlimited placement of held blocks. |
| `uxmessentials.butcher.use` | `op` | `/butcher [radius]`: purge nearby mobs (audit-logged). |
| `uxmessentials.entitycount.use` | `op` | `/entitycount [radius]`: tally nearby entities by type for lag diagnosis. |
| `uxmessentials.kill.use` | `op` | `/kill [player\|entity]`: kill a target (audit-logged). |
| `uxmessentials.killall.use` | `op` | `/killall [type]`: purge entities world-wide (audit-logged). |
| `uxmessentials.kittycannon.use` | `op` | `/kittycannon`: launch an exploding cat (audit-logged). |
| `uxmessentials.remove.use` | `op` | `/remove <type> [radius]`: remove entities by type (audit-logged). |
| `uxmessentials.spawner.use` | `op` | `/spawner <type>`: set a spawner's mob type (audit-logged). |
| `uxmessentials.spawnmob.use` | `op` | `/spawnmob <type> [amount]`: spawn mobs (audit-logged). |
| `uxmessentials.copyinv.use` | `op` | `/copyinv <player>`: copy a player's inventory into yours. |
| `uxmessentials.disposal.use` | `op` | `/disposal` (alias `/trash`): open a throwaway GUI. |
| `uxmessentials.enderclear.use` | `op` | `/enderclear` (alias `/clearec`) `[player]`: clear an ender chest. |
| `uxmessentials.endercopy.use` | `op` | `/endercopy <player>`: copy a player's ender chest into yours. |
| `uxmessentials.workstation.anvil` | `op` | `/anvil`: open a virtual anvil. |
| `uxmessentials.workstation.cartography` | `op` | `/cartography`: open a virtual cartography table. |
| `uxmessentials.workstation.enderchest` | `op` | `/enderchest` (alias `/echest`): open your ender chest. |
| `uxmessentials.workstation.furnace` | `op` | `/furnace`: open a virtual furnace. |
| `uxmessentials.workstation.grindstone` | `op` | `/grindstone`: open a virtual grindstone. |
| `uxmessentials.workstation.loom` | `op` | `/loom`: open a virtual loom. |
| `uxmessentials.workstation.others` | `op` | Open any virtual workstation on another player with the `[player]` target form. |
| `uxmessentials.workstation.smithingtable` | `op` | `/smithingtable`: open a virtual smithing table. |
| `uxmessentials.workstation.stonecutter` | `op` | `/stonecutter`: open a virtual stonecutter. |
| `uxmessentials.workstation.workbench` | `op` | `/workbench` (alias `/craft`): open a virtual crafting table. |
| `uxmessentials.antioch.use` | `op` | `/antioch` (alias `/grenade`): throw a primed TNT grenade (audit-logged). |
| `uxmessentials.beezooka.use` | `op` | `/beezooka` (alias `/beecannon`): launch an angry bee (audit-logged). |
| `uxmessentials.break.use` | `op` | `/break`: instantly break the block you are looking at (audit-logged). |
| `uxmessentials.fireball.use` | `op` | `/fireball`: launch a fireball (audit-logged). |
| `uxmessentials.lightning.use` | `op` | `/lightning` (alias `/smite`) `[player]`: strike lightning (audit-logged). |
| `uxmessentials.nuke.use` | `op` | `/nuke [player]`: rain lightning over an area (audit-logged). |
| `uxmessentials.tree.use` | `op` | `/tree <type>`: generate a tree of the given type where you are looking (audit-logged). |
| `uxmessentials.powertool.toggle` | `op` | `/powertooltoggle`: enable/disable your powertool bindings. |
| `uxmessentials.powertool.use` | `op` | `/powertool <command>` (alias `/pt`): bind a command to the held item. |
| `uxmessentials.time.alias` | `op` | `/day` / `/night` quick time aliases. |
| `uxmessentials.time.use` | `op` | `/time <set\|add> <value>`: per-world time. |
| `uxmessentials.weather.alias` | `op` | `/sun` / `/rain` / `/thunder` quick weather aliases. |
| `uxmessentials.weather.use` | `op` | `/weather <clear\|rain\|thunder> [duration]`. |

`/give`, `/enchant` and `/spawnmob` also layer a per-type opt-out family: `uxmessentials.itemworld.give.<material>`, `.enchant.<enchant>`, `.spawnmob.<mob>`: see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes). The module's reload tier is `uxmessentials.module.itemworld`.

---

## Holograms

Named, world-placed native-Display holograms. The whole `/hologram` surface is gated as one operator tool.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.hologram.use` | `op` | `/hologram` to create, edit, move, list and delete native-Display holograms. |
| `uxmessentials.hologram.create` | `true` | `/hologram` create and copy. Held by default alongside the base node; negate it to leave an operator editing only the holograms that already exist. |
| `uxmessentials.hologram.delete` | `true` | `/hologram` delete. Held by default alongside the base node; the capability most worth negating for build staff. |
| `uxmessentials.hologram.move` | `true` | `/hologram` movehere, moveto, center, teleport and rotate: change where a hologram sits. |
| `uxmessentials.hologram.appearance` | `true` | `/hologram` billboard, background, glow, opacity, shadow, linewidth, viewrange, alignment, seethrough, growup, item, block, head and entity: change how a hologram looks. |
| `uxmessentials.hologram.visibility` | `true` | `/hologram` visibility, visibilitydistance, show, hide, blacklist and unblacklist: change who sees a hologram. |
| `uxmessentials.hologram.action` | `true` | `/hologram` action and clickcommand: change what a hologram runs when clicked. |
| `uxmessentials.hologram.view` | `true` | `/hologram` list, info and nearby: read-only inspection. |
| `uxmessentials.hologram.edit` | `true` | The line and page content of a hologram (addline, setline, insertline, removeline, page, leaderboard, linknpc, unlinknpc). |
| `uxmessentials.holograms.gui` | `op` | `/hologram` (no args) opens the holograms management GUI. |
| `uxmessentials.module.holograms` | `op` | Hot-reload / inspect the holograms module (native-Display holograms behind `/hologram`). |

A hologram gated with `visibility <name> PERMISSION <node>` uses an **operator-chosen** node (any node your permission plugin manages): it is not a plugin-declared node and has no fixed entry.

---

## NPC

Server-wide packet fake-player/entity NPCs. The `/npc` command is an operator surface gated as a whole.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.npc` | `op` | Hot-reload / inspect the npc module (server-wide fake-player NPCs behind `/npc`). |
| `uxmessentials.npc.admin` | `op` | `/npc` to create, delete, list, move, re-skin, and bind the click command of fake-player NPCs. |
| `uxmessentials.npc.create` | `true` | `/npc` create and `/npc` copy. Held by default alongside the admin node; negate it to leave an operator editing only the NPCs that already exist. |
| `uxmessentials.npc.delete` | `true` | `/npc` delete. Held by default alongside the admin node; the capability most worth negating for build staff. |
| `uxmessentials.npc.move` | `true` | `/npc` movehere, moveto, teleport, center and fix: change where an NPC stands. |
| `uxmessentials.npc.appearance` | `true` | `/npc` skin, skinslim, type, equip, glow, pose, scale and displayname: change how an NPC looks. |
| `uxmessentials.npc.action` | `true` | `/npc` command and `/npc` action: change what an NPC runs when clicked. |
| `uxmessentials.npc.view` | `true` | `/npc` list, info, nearby and help: read-only inspection. |
| `uxmessentials.npc.edit` | `true` | Every remaining `/npc` setting (data, state, cooldown, mirror, collidable, showintab, view and turn distance). |
| `uxmessentials.npc.gui` | `op` | `/npc` (no args) opens the NPC management GUI. |
| `uxmessentials.npc.limit.<n>` | `quota` | How many NPCs you may own; the largest tier held wins. |

---

## Menus

The operator surface over the built-in [menu engine](../menus/engine.md).

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.menu.admin` | `op` | `/menu` reload to re-read the menus/ folder from disk. |
| `uxmessentials.menu.editor` | `op` | `/menu` editor (and the `/uxmess` gui hub entry) to create, duplicate, rename and delete custom menus in-game. |
| `uxmessentials.menu.open.others` | `op` | `/menu` open `<menu> <player>`: open a custom menu for somebody else. |
| `uxmessentials.menu.use` | `true` | `/menu` open `<name>` to open an operator custom menu, and `/menu` list to see the loaded menus. |
| `uxmessentials.module.custommenus` | `op` | Hot-reload / inspect the custommenus module (operator custom menus behind `/menu`). |

---

## Vote

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.vote` | `op` | Hot-reload / inspect the vote module (Votifier vote rewards and vote party). |
| `uxmessentials.vote.admin` | `op` | `/vote` admin givevote `<player> [amount]` and `/vote` admin reset `<player>`: inject or clear votes for any player. |
| `uxmessentials.vote.testreward` | `op` | `/vote` testreward to simulate a vote for yourself and verify the configured rewards. |
| `uxmessentials.vote.top` | `true` | `/vote` top `[period]` to see the vote leaderboard for the given period. |
| `uxmessentials.vote.use` | `true` | `/vote` to see the server's vote links. |
| `uxmessentials.voteparty.admin` | `op` | `/voteparty` force\|set\|add: force the party or adjust the party counter. |
| `uxmessentials.voteparty.use` | `true` | `/voteparty` to see progress towards the next vote party. |

---

## Scoreboard, Tablist & Nametags

The **tablist** and **nametags** modules register no commands: both are packet/config-driven.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.scoreboard` | `op` | Hot-reload / inspect the scoreboard module (per-player sidebar and tablist on uxmlib-hud). |
| `uxmessentials.scoreboard.gui` | `true` | `/scoreboard` gui (and the scoreboard entry on the `/uxmess` gui hub) to open the per-player scoreboard settings panel: the show/hide toggle. |
| `uxmessentials.scoreboard.use` | `true` | `/scoreboard` (alias `/sb`) to toggle whether you see the scoreboard display. |
| `uxmessentials.module.tablist` | `op` | Hot-reload / inspect the tablist module (the player list header, footer and rows). |
| `uxmessentials.module.nametags` | `op` | Hot-reload / inspect the nametags module (the name shown above a player). |

---

## Discord Linking

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.discord.gui` | `true` | `/discordlink` gui (and the discordlink entry on the `/uxmess` gui hub) to open the link-status panel: your binding, a generate-code button, and a confirm-gated unlink. |
| `uxmessentials.discord.link` | `true` | `/discordlink` (issue a code), `/discordlink` status (show your binding), and `/discordunlink` (remove it): all act only on your own account. |
| `uxmessentials.module.discordlink` | `op` | Hot-reload / inspect the discordlink module (account linking and Discord notifications). |

The `/link` redemption itself runs in the optional Discord bridge jar; the in-game commands work whether or not the bridge is installed.

---

## Sitting & Poses

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.bellyflop.use` | `true` | `/bellyflop`: flop onto your front where you stand. |
| `uxmessentials.crawl.use` | `true` | `/crawl`: crawl through a one-block gap; run again to stand up. |
| `uxmessentials.lay.use` | `true` | `/lay`: lie down on your back where you stand. |
| `uxmessentials.module.poses` | `op` | Hot-reload / inspect the poses module (built-in GSit-parity sitting and posing). |
| `uxmessentials.playersit.use` | `true` | Right-click another player to sit on them (the stacking pose), when player-sit is enabled. |
| `uxmessentials.poses.gui` | `true` | `/poses` (or `/poses` gui): open your personal poses settings & status panel. |
| `uxmessentials.poses.toggle` | `true` | `/poses` toggle: allow or refuse other players sitting on you. |
| `uxmessentials.sit.use` | `true` | `/sit` and right-click-to-sit: sit down where you stand or on a sittable block. |
| `uxmessentials.spin.use` | `true` | `/spin`: sit and spin in place. |

The optional numbered node `uxmessentials.poses.cooldown.<seconds>` sets an anti-spam wait between starting poses (an open value space, like `kit.cooldown.<seconds>`: see [Numbered and Tiered Nodes](#numbered-and-tiered-nodes)). See the [Sitting & Poses](../features/poses.md) guide for how each pose works.

---

## Survival

Opt-in gameplay mechanics, each an independently toggleable sub-feature. A mechanic carries a **use** node `uxmessentials.survival.<mechanic>` (whether it acts for the player), and the seven with a personal `/command` toggle also carry `uxmessentials.survival.<mechanic>.toggle` (whether the player may switch it, from the command or the `/survival` panel). All are self-service (`default: true`): the module itself ships off, and once an operator turns it on the mechanics act for everyone and a player opts out.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.survival` | `op` | Hot-reload / inspect the survival module (opt-in gameplay mechanics). |
| `uxmessentials.survival.autopickup` | `true` | Auto-pickup acts for you: mined drops go straight to your inventory. |
| `uxmessentials.survival.autopickup.toggle` | `true` | `/autopickup`: switch your personal auto-pickup on or off. |
| `uxmessentials.survival.autosell` | `true` | Auto-sell acts for you: priced drops are sold for coin as you mine. |
| `uxmessentials.survival.autosell.toggle` | `true` | `/autosell`: switch your personal auto-sell on or off. |
| `uxmessentials.survival.autosmelt` | `true` | Auto-smelt acts for you: ores are smelted as you mine them. |
| `uxmessentials.survival.autosmelt.toggle` | `true` | `/autosmelt`: switch your personal auto-smelt on or off. |
| `uxmessentials.survival.autotool` | `true` | Auto-tool acts for you: the best tool swaps to hand as you mine. |
| `uxmessentials.survival.autotool.toggle` | `true` | `/autotool`: switch your personal auto-tool on or off. |
| `uxmessentials.survival.farmassist` | `true` | Right-click a mature crop to harvest and replant it, spending one seed. |
| `uxmessentials.survival.farmprotect` | `true` | Farmland protection acts for you: you will not trample your crops. |
| `uxmessentials.survival.farmprotect.toggle` | `true` | `/farmprotect`: switch your personal farmland protection on or off. |
| `uxmessentials.survival.gui` | `true` | `/survival`: open your personal survival mechanics settings panel. |
| `uxmessentials.survival.treefeller` | `true` | Tree-feller acts for you: break one log to fell the whole trunk. |
| `uxmessentials.survival.treefeller.toggle` | `true` | `/treefeller`: switch your personal tree-feller on or off. |
| `uxmessentials.survival.veinminer` | `true` | Veinminer acts for you: break one block to mine the connected vein. |
| `uxmessentials.survival.veinminer.toggle` | `true` | `/veinminer`: switch your personal veinminer on or off. |

Fast leaf decay, the anvil unlocker, one-player sleep and head drops are world-side effects with no permission node: they're governed only by their config blocks. See the [Survival Mechanics](../features/survival.md) guide for what each mechanic does.

---

## Security

Account security: an optional second factor a player enrols themselves, join verification, op-command protection, and a same-IP alt / ClientID guard. **Not** a login system: there's no password or first-join gate, and on an offline-mode server it waits for your login plugin rather than replacing it. The two self-service nodes ship `true`; the staff reads and the bypass default to `op`. See the [Account Security](../features/security.md) guide for the flows.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.security` | `op` | Hot-reload / inspect the security module (2FA, op-protection, IP/alt guard). |
| `uxmessentials.security.2fa` | `true` | /2fa: set up, confirm or disable an authenticator second factor on your own account. |
| `uxmessentials.security.admin` | `op` | `/security`: inspect and manage another player's second factors. |
| `uxmessentials.security.alts` | `op` | `/ipalts`: list the accounts that share an IP with a player. |
| `uxmessentials.security.alts.notify` | `op` | Receive the staff notice when a join shares an IP with other accounts or reports a flagged client. |
| `uxmessentials.security.bypass` | `op` | Exempt from the two-factor join verification and op-command re-auth checks. |
| `uxmessentials.security.clientinfo` | `op` | `/clientinfo`: show the client brand a player reported. |
| `uxmessentials.security.force` | `op` | `/security` force `<player>`: force a player to re-verify their second factor on their next action or join. |
| `uxmessentials.security.pin` | `true` | `/pin`: set, change or remove a numeric PIN second factor on your own account. |
| `uxmessentials.security.pin.required` | `false` | Must set a PIN before playing. |
| `uxmessentials.security.reset` | `op` | `/security` reset `<player> [totp\|pin\|all]`: clear a factor a player can no longer prove. |

---

## Command Control

Command whitelist / blacklist gating, tab-completion filtering, plugin-hide, and the namespace-bypass block: the PlHidePro / CommandWhitelist feature set. It ships enabled but inert (a blacklist with empty lists, plugin-hide off), so both nodes default to `op` and gate nobody until you name commands. See the [Command Control](../features/commandcontrol.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.commandcontrol.bypass` | `op` | Exempt from the command whitelist/blacklist gate and the tab-completion / plugin-hide scrub. |
| `uxmessentials.commandcontrol.channelhide.bypass` | `op` | Exempt from the plugin-channel hider - the full channel-registration list is sent to this player. |
| `uxmessentials.commandcontrol.spam.bypass` | `op` | Exempt from the command-spam rate limiter - commands are never counted and no spam action fires. |
| `uxmessentials.commandcontrol.viewplugins` | `op` | See the plugin-listing / help commands (`/plugins`, `/pl`, `/help`, ...) hidden by the plugin-hide feature. |
| `uxmessentials.module.commandcontrol` | `op` | Hot-reload / inspect the commandcontrol module (command whitelist, tab-completion filter, plugin-hide). |

---

## Villagers

Villager trade management: trades that never lock out, a restock timer, a staff trade editor, click-to-trade, a villager saver, pickup, follow, and leashing. The module ships **disabled**, so all six action nodes default to `op` and grant nothing until you turn the module on and then the matching feature with it. The four trade-availability switches (infinite trading, restock, instant restock, disable trades) take no permission: they're config-only. See the [Villagers](../features/villagers.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.villagers` | `op` | Hot-reload / inspect the villagers module (villager trade management). |
| `uxmessentials.villagers.bucket` | `op` | Sneak-right-click a villager to pick it up into a captured-villager item, and place it back later. |
| `uxmessentials.villagers.follow` | `op` | `/villager` follow: toggle whether the villager you are looking at pathfinds after you. |
| `uxmessentials.villagers.leash` | `op` | Right-click a villager with a lead to leash it, when leashing is enabled. |
| `uxmessentials.villagers.manager` | `op` | `/villager` manager: open and edit the trades of the villager you are looking at. |
| `uxmessentials.villagers.protect` | `op` | `/villager` protect: toggle whether the villager you are looking at is protected from death and despawn. |
| `uxmessentials.villagers.trade` | `op` | Open a villager's trade window directly on right-click, when click-to-trade is enabled. |
| `uxmessentials.villagers.use` | `op` | `/villager`: the root command's base node; its subcommands each gate further on their own node. |

---

## Inventory Rollback

Inventory snapshot and restore: the plugin freezes a player's inventory on death and (by config) on logout, keeps a bounded history of those snapshots in the database (never PDC, so they survive a world rollback), and lets staff browse and restore them from a GUI. The module ships **disabled**; turn it on and it captures on death and logout straight away. A restore acts on a live inventory, so the target must be online. See the [Inventory Rollback](../features/invrollback.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.invrollback.export` | `op` | `/invrestore` export `<player> <index>` packs a snapshot into shulker boxes and gives them to you. |
| `uxmessentials.invrollback.restore` | `op` | `/invrestore <player>` opens the inventory-snapshot restore GUI for a player. |
| `uxmessentials.invrollback.teleport` | `op` | `/invrestore` tp `<player> <index>` teleports you to where the snapshot was captured. |
| `uxmessentials.module.invrollback` | `op` | Hot-reload / inspect the invrollback module (inventory snapshots and restore). |

---

## Regions

WorldGuard region management: a menu over an installed WorldGuard to browse regions,
define cuboids, cycle their state flags, and edit their member/owner rosters. WorldGuard is
a **soft-dependency**: with it absent the module is inert and every command answers
*"WorldGuard not installed"* (WorldEdit is a further optional soft-depend, used only for
selection-based `/regions create`). Region management is staff work, so all five action
nodes default to `op`. See the [Regions](../features/regions.md) guide.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.regions` | `op` | Hot-reload / inspect the regions module (WorldGuard region management). |
| `uxmessentials.regions.admin` | `op` | `/regions` priority `<id> <value>`: set a WorldGuard region priority. |
| `uxmessentials.regions.create` | `op` | `/regions` create `<id>` (and `/regions` pos1\|pos2): define a cuboid WorldGuard region. |
| `uxmessentials.regions.flags` | `op` | `/regions` flags `<id>`: open the per-region flag editor GUI. |
| `uxmessentials.regions.list` | `op` | `/regions [world]`: open the WorldGuard region-list GUI for a world. |
| `uxmessentials.regions.members` | `op` | `/regions` members `<id>` and `/regions` addmember\|addowner `<id> <player>`: manage a region roster. |

---

## Server Tweaks

A grab-bag of silent server-side infrastructure tweaks: a custom F3 brand, a console-log
filter, an unsigned-chat option, and a SignedVelocity backend companion. The module is
**config-only**: it registers no commands, so it has **no action permission nodes**. Every
tweak defaults off and is switched entirely from `modules/servertweaks/config.conf`. See
the [Server Tweaks](../features/server-tweaks.md) guide. The module's reload tier is
`uxmessentials.module.servertweaks`.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.module.servertweaks` | `op` | Hot-reload / inspect the servertweaks module (the small server-behaviour switches). |

---

## Shared kernel

The kernel owns no feature commands, but carries two cross-cutting self-service verbs.

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.admin` | `op` | Access to the `/uxmess` administration root. |
| `uxmessentials.admin.backup` | `op` | `/backup` to snapshot the plugin data directory on demand. |
| `uxmessentials.admin.import` | `op` | `/uxmess` import `<plugin>`: run the one-shot data import from another essentials plugin. |
| `uxmessentials.admin.permissions` | `op` | `/uxmess` permissions `[area] [page]` and `/uxmess` permissions export: read the permission catalogue in game or write it to a file. |
| `uxmessentials.admin.placeholders` | `op` | `/uxmess` placeholders `[area] [page]` and `/uxmess` placeholders export: read the placeholder catalogue in game or write it to a file. |
| `uxmessentials.admin.reload` | `op` | Reload all modules via `/uxmess` reload. |
| `uxmessentials.cooldown.<feature>.<seconds>` | `tier` | The wait between uses of one rate-limited feature, in seconds; the shortest tier held wins and 0 removes the wait. |
| `uxmessentials.cooldown.bypass.<feature>` | `label` | Skip the cooldown on one rate-limited feature entirely (tp, rtp, kit, poses). |
| `uxmessentials.gui` | `op` | `/uxmess` gui to open the module management hub. |
| `uxmessentials.help` | `true` | `/help` to list the commands you can use. |
| `uxmessentials.lang.use` | `true` | `/lang` to set or clear your personal language override. |
| `uxmessentials.update.notify` | `op` | Receive the join-time notice when a newer plugin version is available. |

---

## Numbered and Tiered Nodes

Some limits are *values*, not yes/no flags: how many homes a player may own, how long their teleport cooldown is. Instead of hard-coding these per group, uxmEssentials encodes the value **in the node** and reads the best-matching one the player holds, falling back to a config default.

| Node family | Kind | Meaning |
|-------------|------|---------|
| `uxmessentials.cooldown.<feature>.<seconds>` | value, see the rule | The wait between uses of one rate-limited feature, in seconds; the shortest tier held wins and 0 removes the wait. |
| `uxmessentials.cooldown.bypass.<feature>` | boolean, per name | Skip the cooldown on one rate-limited feature entirely (tp, rtp, kit, poses). |
| `uxmessentials.rtp.radius.<blocks>` | value, largest wins | How far from the world centre `/rtp` may drop you; the largest tier held wins. |
| `uxmessentials.tp.warmup.<seconds>` | value, see the rule | The stand-still countdown before a teleport runs, in seconds; the shortest tier held wins and 0 removes it. |
| `uxmessentials.home.limit.<n>` | value, largest wins | How many homes you may keep; the largest tier held wins. |
| `uxmessentials.warp.use.<warp>` | boolean, per name | Use one warp, when that warp is configured to require a permission. |
| `uxmessentials.pwarp.limit.<n>` | value, largest wins | How many player warps you may own; the largest tier held wins. |
| `uxmessentials.world.<world>` | boolean, per name | Enter one world that is configured as restricted. |
| `uxmessentials.vault.amount.<n>` | value, largest wins | How many vaults you may open; the largest tier held wins. |
| `uxmessentials.vault.size.<rows>` | value, largest wins | How many rows each of your vaults holds; the largest tier held wins. |
| `uxmessentials.economy.currency.<currency>` | boolean, per name | Use one currency that is configured to require a permission. |
| `uxmessentials.economy.salary.amount.<amount>` | value, largest wins | The periodic salary you are paid; the largest tier held wins. |
| `uxmessentials.moderation.<sanction>.maxduration.<seconds>` | value, see the rule | The longest ban, mute or jail you may hand out, in seconds; the largest tier held wins. |
| `uxmessentials.vanish.see.level<n>` | value, largest wins | How deeply you see: a viewer sees a vanished player when their see level reaches that player's use level; the largest level held wins and plain .see is level 1. |
| `uxmessentials.vanish.use.level<n>` | value, largest wins | How deeply you vanish: only a viewer whose see level reaches this level finds you; the largest level held wins and plain .use is level 1. |
| `uxmessentials.communication.info.<page>` | boolean, per name | Read one information page, such as `/motd` or `/rules`. |
| `uxmessentials.itemworld.enchant.<enchantment>` | boolean, per name | Apply one specific enchantment through `/enchant`, when per-enchantment gating is switched on. |
| `uxmessentials.itemworld.give.<item>` | boolean, per name | Give one specific item through `/give`, when per-item gating is switched on. |
| `uxmessentials.itemworld.spawnmob.<mob>` | boolean, per name | Spawn one specific mob through `/spawnmob`, when per-mob gating is switched on. |
| `uxmessentials.npc.limit.<n>` | value, largest wins | How many NPCs you may own; the largest tier held wins. |
| `uxmessentials.kit.<kit>` | boolean, per name | Claim one kit. |
| `uxmessentials.kit.cooldown.<kit>.<seconds>` | value, see the rule | The wait between claims of one kit, in seconds; the shortest tier held wins. |

### How they resolve

Every value-bearing node passes through the shared **`Permissions`** port and a single **`QuotaNodeReducer`**, so the semantics are identical across contexts:

- **Quota families** (`home.limit`, `vault.amount`, `vault.size`, `pwarp.limit`, `*.maxduration`) reduce to the **maximum**: more is better, the most generous node wins.
- **Cooldown / warmup families** (`uxmessentials.cooldown.<feature>.<seconds>`, `uxmessentials.kit.cooldown.<kit>.<seconds>`, `uxmessentials.tp.warmup.<seconds>`) reduce to the **minimum**: less is better; `0` means "no wait".
- **`-1` is the unlimited sentinel** for quota families (a `home.limit.-1` means no quota at all). It is never used for cooldown/warmup families, where `0` is "no wait".
- A player with **no** numbered node and no meta falls back to the per-context **config default**: never to "unlimited".

A numbered node may carry an **optional `<world>` segment** between the family and the value, so a quota or cooldown can differ per destination world:

```
uxmessentials.home.limit.creative.20
uxmessentials.tp.warmup.nether.0
uxmessentials.cooldown.warp.nether.5
```

The unscoped form applies everywhere; the world-scoped form applies only when that world is the one being resolved against. Matching scoped and unscoped nodes are simply folded into the same reducer.

<Callout type="note" title="LuckPerms meta is an alternative, never a requirement">

When LuckPerms is installed, these same quotas can also be expressed as LuckPerms **meta** (for example `meta.home-limit`, `meta.warp-cooldown`). A meta value takes precedence over the numbered node when present. With no permission plugin, only the numbered node and the config default apply: the numbered form is the portable baseline.

</Callout>

Because the value spaces (`<n>`, `<seconds>`, `<rows>`, and so on) are open, a family is never registered with the server as a node of its own; operators grant the specific instances they need. The families themselves are listed above and by `/uxmess permissions`.

---

## Bypass Nodes

Bypass nodes let a trusted player cut through a gate (a cooldown, warmup, cost, lock, or restriction) that would normally reject them. They default to `op` unless the row says otherwise: the marker nodes that only *opt a holder out* of something default to `false`, so nobody holds them by accident.

| Node | What it lifts |
|------|---------------|
| `uxmessentials.cooldown.bypass.<feature>` *(a family, one node per name)* | Skip the cooldown on one rate-limited feature entirely (tp, rtp, kit, poses). |
| `uxmessentials.tp.warmup.bypass` | Start teleports with no warmup, immune to move-cancel. |
| `uxmessentials.home.bypass.cost` | Skip the per-action economy cost for home create, relocate, and teleport actions. |
| `uxmessentials.home.bypass.unsafe` | Skip the unsafe-destination confirm when teleporting to a home via the GUI. |
| `uxmessentials.warp.bypass.lock` | Use a locked warp regardless of its lock state. |
| `uxmessentials.warp.bypass.password` | Use a password-protected warp without entering its password. |
| `uxmessentials.warp.bypass.safety` | Use a warp whose destination fails the safety check. |
| `uxmessentials.pwarp.bypass.ban` | Enter a player warp you are banned from (skips the ban check on `/pwarp`). |
| `uxmessentials.pwarp.bypass.password` | Enter a password-protected player warp without the password (skips the check on `/pwarp`). |
| `uxmessentials.pwarp.bypass.whitelist` | Enter a whitelist-only player warp without being on the whitelist (skips the check on `/pwarp`). |
| `uxmessentials.pwarp.bypass.cost` | Use a priced player warp without paying its entry cost (skips the charge on `/pwarp`). |
| `uxmessentials.pwarp.bypass.safety` | Use a player warp whose destination is unsafe (skips the safe-landing check on `/pwarp`). |
| `uxmessentials.world.access.bypass` | Enter a restricted world without holding that world's own entry node. |
| `uxmessentials.world.gamemode.bypass` | Keep your own game mode in a world that forces one on entry. |
| `uxmessentials.vault.bypass-blacklist` | Store any item in a vault, ignoring the blacklist-materials list (items are not returned). |
| `uxmessentials.economy.baltop.exempt` *(default `false`)* | Marks the holder as hidden from every `/baltop` leaderboard. |
| `uxmessentials.economy.bypasscmdcost` | Skip the configured per-command economy charge (command-costs in economy.conf). |
| `uxmessentials.economy.tax.bypass` | Send a `/pay` without the configured transfer tax being deducted. |
| `uxmessentials.moderation.lockdown.bypass` | Join the server while it is locked down (`/lockdown`). Held by staff who must stay reachable during a lockdown. |
| `uxmessentials.moderation.exempt` | Cannot be muted/jailed/tempbanned/kicked/warned/IP-banned/frozen by lower staff. |
| `uxmessentials.security.bypass` | Exempt from the two-factor join verification and op-command re-auth checks. |
| `uxmessentials.commandcontrol.bypass` | Exempt from the command whitelist/blacklist gate and the tab-completion / plugin-hide scrub. |
| `uxmessentials.commandcontrol.channelhide.bypass` | Exempt from the plugin-channel hider - the full channel-registration list is sent to this player. |
| `uxmessentials.commandcontrol.spam.bypass` | Exempt from the command-spam rate limiter - commands are never counted and no spam action fires. |
| `uxmessentials.communication.chat.bypass` | Keep chatting while public chat is locked by `/togglechat`. |
| `uxmessentials.communication.clearchat.exempt` *(default `false`)* | Keep your chat scrollback when staff run `/clearchat`. |
| `uxmessentials.kit.cooldown.bypass` | Skip kit cooldowns and re-claim one-time kits. |

---

## Module Gates and Admin Roots

### `uxmessentials.module.<id>`: per-module reload tier

Every bounded context is a first-class feature module. Each owns a node tier `uxmessentials.module.<id>` (default `op`): **not** the feature's runtime permission, but the per-module **administrative** gate: who may inspect and hot-reload that one module via `/uxmess reload <id>` without holding the blanket `uxmessentials.admin.reload`. `<id>` is the module id used everywhere else (config path `modules.<id>`, the reload command, the tier node):

```
commandcontrol  communication  custommenus  discordlink  economy  holograms
homes  invrollback  itemworld  kits  messaging  moderation
nametags  npc  playerstate  playerwarps  poses  presence
ranks  regions  scoreboard  security  servertweaks  staff
survival  tablist  teleport  trade  vanish  vaults
villagers  vote  warps  worlds
```

`/uxmess reload <module>` accepts either `uxmessentials.admin.reload` (blanket) or the matching `uxmessentials.module.<id>` (delegated), so you can hand a junior staffer reload rights over one module without the keys to the whole plugin.

### `/uxmess` admin tree

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.admin` | `op` | `/uxmess` read-only subcommands: `status`, `doctor`, help |
| `uxmessentials.admin.reload` | `op` | `/uxmess reload` (all or one module) |
| `uxmessentials.admin.import` | `op` | `/uxmess import <source>`: the one-shot idempotent importer |
| `uxmessentials.admin.backup` | `op` | `/backup`: snapshot the plugin data directory |
| `uxmessentials.admin.permissions` | `op` | `/uxmess permissions [area] [page]` and `/uxmess permissions export`: read this page in game, or write it to `permissions.md` in the plugin folder |
| `uxmessentials.gui` | `op` | `/uxmess gui`: the central management hub |

### Management-GUI nodes

The management hub gathers each module's management GUI behind one menu. Each module registers its entry under `uxmessentials.<module>.gui` (for example `uxmessentials.holograms.gui`, `uxmessentials.npc.gui`, `uxmessentials.economy.admin`). Each defaults to `op`, the hub shows a viewer only the entries whose node they hold, and a module's own `/<module>` GUI entry point opens directly for a holder of the same node, so you can delegate one module's panel without granting the rest.

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

- [Command Overview](../commands/overview.md): how commands are registered, aliased, renamed, and disabled
- [LuckPerms Integration](../integrations/luckperms.md): meta-based quotas and group resolution
- [Developer API](../developer/overview.md): integrate with uxmEssentials at runtime
