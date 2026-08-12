---
title: Query API
order: 1640
description: 'Reading uxmEssentials state: homes, balances, punishments, presence, worlds.'
---

Events tell you what uxmEssentials did. Queries answer what is true now: how many homes a player owns, what
they hold, whether they are banned, who is away, which worlds are loaded.

Every surface hangs off the front door, and every one arrives as an `Optional`:

```java
UxmEssentialsApi api = UxmEssentialsApi.get();
if (api == null) {
    return;
}

api.homes().ifPresent(homes ->
    homes.list(player.getUniqueId()).thenAccept(owned ->
        getLogger().info(player.getName() + " has " + owned.size() + " homes")));
```

An empty `Optional` means the module is switched off. That is not the same as the answer being nothing, and it is
worth telling apart: one is a server that will never answer, the other is a player who has not set a home yet. Ask
`api.isModuleEnabled("homes")` if you want to say so in a log line.

## Threading

Two shapes, and the return type tells you which is which.

| Returns | Means | Call it from |
|---|---|---|
| `CompletableFuture<T>` | the answer is in the database | anywhere; the read already runs off the tick thread |
| a plain value | the answer is already in memory | anywhere, including a listener on the tick thread |

**Never call `join()` or `get()` on the main thread.** The read has already been moved off it for you; blocking the
tick thread to wait for it is the one way to turn a cheap query into a stall. Chain instead:

```java
api.economy().ifPresent(economy ->
    economy.balance(playerId)
        .thenAccept(money -> getLogger().info("holds " + money.amount() + " " + money.currency()))
        .exceptionally(failure -> {
            getLogger().warning("balance read failed: " + failure.getMessage());
            return null;
        }));
```

If the continuation touches the Bukkit API, hop back to the tick thread yourself, the way you would after any async
work:

```java
economy.balance(playerId).thenAccept(money ->
    getServer().getGlobalRegionScheduler().execute(this, () -> player.sendMessage("You hold " + money.amount())));
```

Independent reads should not queue behind each other. Start both, then combine:

```java
CompletableFuture<UxmMoney> balance = economy.balance(playerId);
CompletableFuture<List<UxmBaltopEntry>> top = economy.top(3);

balance.thenAcceptBoth(top, (held, richest) -> { /* ... */ });
```

<Callout type="warning" title="Queries read, they never write">

Nothing here charges a player, stamps a cooldown, marks mail read or spends a `/back` point. Asking whether a kit
is claimable does not claim it, and reading a mailbox does not clear the unread count. Anything that changes
state is a use case with rules attached, and a read surface has no business going around them.

</Callout>

## Offline players

Everything database-backed answers for a player who is offline, which is usually when the answer matters: mail
waiting for somebody, a ban to enforce, a balance to show on a web panel.

The memory-backed surfaces cannot, and say so honestly. `presence().of(uuid)` is empty for a player who is not
online, because nobody is at that keyboard to be away from it.

Players are named by UUID throughout. A name is not an identity: it changes, and on an offline-mode server it is not
unique. Resolve a name to a UUID with Bukkit before you ask.

## Homes

`api.homes()` &rarr; `UxmHomesQuery`

| Method | Answers |
|---|---|
| `list(playerId)` | every home the player owns, in slot order |
| `get(playerId, slot)` | one home by slot, counting from zero |
| `count(playerId)` | how many they have set |
| `limit(playerId)` | how many they may set; empty means unlimited |

`UxmHome` carries the slot, the location, the label and icon the owner chose, whether it is public, and when it was
set and last changed. `slotNumber()` is the number the player sees (counting from one) and `displayName()` is the
label if there is one, otherwise that number.

## Warps

`api.warps()` &rarr; `UxmWarpsQuery`

| Method | Answers |
|---|---|
| `list()` | every warp on the server |
| `get(name)` | one warp by name |
| `visibleTo(playerId)` | the warps this player would see in `/warps` |
| `exists(name)` | whether a warp has that name |
| `averageRating(name)` | its average rating, or zero when nobody has rated it |

`visibleTo` runs the same filter the command does: permissions, hidden warps, per-world visibility. Use it rather
than filtering `list()` yourself, or your menu and the plugin's will disagree.

The rating is its own call on purpose. Ratings are a separate read, and putting one on every warp in a list would
turn one query into as many as there are warps.

## Player warps

`api.playerWarps()` &rarr; `UxmPlayerWarpsQuery`

| Method | Answers |
|---|---|
| `listPublic(page, pageSize)` | a page of the public directory, newest first |
| `get(name)` | one player warp by name |
| `ownedBy(ownerId)` | every warp a player owns, archived ones included |
| `count(ownerId)` | how many they own |
| `limit(ownerId)` | how many they may own; empty means unlimited |

The directory is paged because it can be long. `UxmPlayerWarp` carries its owner, its status (open, archived or
locked), its access rules and its visit count.

## Economy

`api.economy()` &rarr; `UxmEconomyQuery`

| Method | Answers |
|---|---|
| `currencies()` | the configured currency ids, the first of which is the default |
| `balance(playerId)` | what they hold in the default currency |
| `balance(playerId, currency)` | what they hold in one currency; empty for an unknown id |
| `balances(playerId)` | what they hold in every currency |
| `canAfford(playerId, amount)` | whether they hold at least that much in the default currency |
| `canAfford(playerId, amount, currency)` | the same in one currency; `false` for an unknown id |
| `top(limit)` | the richest players, ranked |
| `top(currency, limit)` | the same in one currency |

`canAfford` makes the same comparison the plugin makes before it charges, holding exactly the price included, so a
plugin that checks first and then asks uxmEssentials to charge agrees with it. A negative price is rejected rather
than answered. It is a question about this instant and not a reservation: between the answer and the charge the
player can spend, so keep the gap short and let the charge itself be the authority.

Most servers run one currency, whose id is `default`, and the no-currency forms use it, so a consumer on an ordinary
server never has to think about currencies. `UxmMoney` carries the amount and the currency it is in, so a value can
never be added to the wrong pot by accident.

To move money, register through Vault or Treasury: see [Vault and Treasury](../integrations/vault-treasury.md).

## Kits

`api.kits()` &rarr; `UxmKitsQuery`

| Method | Answers |
|---|---|
| `list()` | the whole catalogue, from memory |
| `get(kitId)` | one kit by id, from memory |
| `cooldownRemaining(playerId, kitId)` | how long until they may claim it; empty when they may now |
| `canClaim(playerId, kitId)` | whether every gate would let them |
| `claimableBy(playerId)` | the kits they could take right now |

The catalogue is configuration held in memory, so those two answer straight away. `canClaim` applies the same gates
the command does (permission, cooldown, one-time claim, stock, price) minus the two that would take something: it
never charges and never spends a one-time claim.

Kit contents are not published. They are Bukkit item stacks, and a plugin that wants to hand a kit out should run
the kit through uxmEssentials rather than rebuild it.

## Vaults

`api.vaults()` &rarr; `UxmVaultsQuery`

| Method | Answers |
|---|---|
| `list(ownerId)` | every vault they have opened, in number order |
| `get(ownerId, index)` | one vault by number, counting from one |
| `count(ownerId)` | how many they have opened |
| `limit(ownerId)` | how many they may open; empty means unlimited |
| `rows(ownerId)` | how many rows each of their vaults holds |

Contents are not published here either, and for the same reason. `UxmVault` is the number, the name and the icon,
which is everything a list or a selector shows and costs no item deserialisation at all.

## Moderation

`api.moderation()` &rarr; `UxmModerationQuery`

| Method | Answers |
|---|---|
| `ban(playerId)` | their active ban, or empty |
| `mute(playerId)` | their active mute, or empty |
| `jail(playerId)` | their active jail sentence, or empty |
| `warns(playerId)` | their warnings that have not expired |
| `history(playerId, limit)` | the most recent punishments on their record |

Every answer is about the present moment. A punishment that has run out is reported as absent rather than as an
expired one, so a plugin enforcing something can act on the answer instead of interpreting it.

`UxmSanction` carries who issued it, the reason, when it was issued and when it ends. A permanent punishment has no
end; so does an online-only jail sentence, which counts down while the player is logged in and therefore has no
wall-clock expiry to publish.

## Presence

`api.presence()` &rarr; `UxmPresenceQuery`. Answers straight away.

| Method | Answers |
|---|---|
| `of(playerId)` | what is known about them, or empty when they are not online |
| `isAfk(playerId)` | whether they are away |
| `afk()` | everyone who is away right now |

`UxmPresence` carries the away flag, the reason if they typed one (an automatic flag has none), and when they last
moved, chatted or ran a command.

## Vanish

`api.vanish()` &rarr; `UxmVanishQuery`. Answers straight away.

| Method | Answers |
|---|---|
| `isVanished(playerId)` | whether they are hidden |
| `vanished()` | everyone hidden right now |
| `levelOf(playerId)` | the level they are hidden at, or zero |
| `canSee(viewerId, targetId)` | whether the viewer may see the target |

**Filtering a list? Use `canSee`.** Vanish has levels: a player is hidden *at* a level, and a viewer sees them when their own level reaches it. On a
server that layers the tiers, `isVanished` alone would hide staff from each other. `canSee` is the question the
tab list, the join messages and the staff tools all ask, and asking it too keeps your list agreeing with theirs.

## Playtime

`api.playtime()` &rarr; `UxmPlaytimeQuery`

| Method | Answers |
|---|---|
| `of(playerId)` | their playtime across today, the week, the month and all time |

Each window is split into time at the keyboard and time away, and is anchored on the server's calendar day rather
than a rolling twenty-four hours, so "today" resets at the server's midnight. `totalConnected()` adds the two if you
want the simple number. A player nobody has sampled reads as zero rather than as absent.

## Player state

`api.playerState()` &rarr; `UxmPlayerStateQuery`. Answers straight away.

| Method | Answers |
|---|---|
| `of(playerId)` | the switches held for them, or empty when they are not online |

`UxmPlayerState` carries the god and flight toggles, the pinned game mode if the plugin is holding one, and the walk
and fly speeds on Bukkit's own `0.0` to `1.0` scale, which is the scale you would set them at.

None of it is written down. The switches are seeded on join, changed by the commands, and dropped on quit, so a
player who is offline reads as empty rather than as a stale snapshot. The game mode is empty for most players: the
module pins one only when somebody set it, and an empty answer says nothing is pinned rather than guessing survival.

## Worlds

`api.worlds()` &rarr; `UxmWorldsQuery`

| Method | Answers |
|---|---|
| `list()` | every world the plugin manages |
| `get(name)` | one managed world by name |
| `isLoaded(name)` | whether it is loaded right now; answers straight away |
| `access(playerId, worldName)` | whether the player would be let in, and why not |

Only managed worlds are here. A world the server has that the operator never took under management is absent rather
than invented, and carries no entry rules, so `access` allows it.

`access` is the same decision `/world` makes: the bypass node, the per-world entry node, the player cap. It decides
only, and reserves nothing, so a world one place from full can still fill between the answer and the teleport.

## Teleport

`api.teleport()` &rarr; `UxmTeleportQuery`. Answers straight away.

| Method | Answers |
|---|---|
| `pendingFor(playerId)` | the requests waiting on them, oldest first |
| `outgoingFrom(playerId)` | the request they have open, or empty |
| `backPoint(playerId)` | where `/back` would return them to |

`UxmTeleportRequest.moverId()` names the player who would actually teleport, which is the whole difference between
`/tpa` and `/tpahere`. A request that has been answered or has run out of time is gone rather than reported stale.

Reading a return point does not spend it: the next `/back` still works.

## Trade

`api.trade()` &rarr; `UxmTradeQuery`. Answers straight away.

| Method | Answers |
|---|---|
| `isTrading(playerId)` | whether they have a trade window open |
| `of(playerId)` | the trade they are in, or empty |
| `open()` | every trade open on this server |

A trade lives in memory for as long as its window is open, so nothing here waits on a database. What is in the two
offers is not published: it changes several times a second and is the participants' business. Listen for
`UxmTradeCompleteEvent` if you need to know what changed hands, which carries the totals once they are final.

The confirmation flags do not stick. Changing an offer clears both, which is the anti-scam rule the window
enforces, so a snapshot read a moment before a change reports the state as it was.

Only trades on this server are visible. A cross-server trade's other half lives on the backend the partner is
connected to.

## Discord links

`api.discordLink()` &rarr; `UxmDiscordLinkQuery`

| Method | Answers |
|---|---|
| `of(playerId)` | the Discord account bound to them, or empty |
| `byDiscordId(discordId)` | the same binding read from the Discord side |
| `isLinked(playerId)` | whether they have one at all |

Both directions are indexed, so neither is the slow one, and every answer is a database row rather than something
held in memory: a binding is account identity and survives a restart and a world rollback alike.

The Discord id is a string because that is how Discord's own API writes a snowflake. An id that is not one answers
empty rather than throwing, which is what a bot passing through whatever a user typed wants.

## Ranks

`api.ranks()` &rarr; `UxmRanksQuery`

| Method | Answers | Waits |
|---|---|---|
| `ladder()` | every rank, in ladder order | no |
| `standingOf(playerId)` | the rank they are on, the one above it, and their prestige level | yes |
| `canRankUp(playerId)` | whether the rung above is within reach right now | yes |

The ladder is parsed configuration held in memory, so it answers on the calling thread. A player who has never
ranked up stands on the first rung: the answer is their standing, not an empty one.

`canRankUp` is the one to read carefully. A rank requirement can name the player's inventory or a placeholder, so
the check runs on the player's own thread and answers `false` for a player who is offline. That is the evaluator's
own rule rather than a shortcut here: a condition that cannot be verified fails closed instead of passing on a
guess.

## Regions

`api.regions()` &rarr; `UxmRegionsQuery`

| Method | Answers |
|---|---|
| `available()` | whether WorldGuard is installed and reachable |
| `in(worldName)` | every region defined in a world |
| `region(worldName, id)` | one region by id |
| `at(location)` | every region covering a point, highest priority first |

A convenience rather than a second source of truth. WorldGuard owns region state and has its own API; this exists so
a plugin that already depends on us can ask "what covers this spot" without taking a second dependency.

Every answer is a future because the region container is read on the server thread rather than on a worker: those
maps are live server state, and reading them from a pool thread is not something WorldGuard's API promises to
survive. `available()` is the exception and answers where you stand, because it is the question you ask first. When
it is false the others answer empty rather than failing.

A region carries its priority, both rosters and the flags it sets. The roster entries are identifiers rather than
players (a uuid, an old name, or a group written `g:name`), because that is what a region holds. The shape is not
published: a region may be a cuboid, a polygon or the whole world, and a corner pair would be a lie for two of the
three.

## Inventory snapshots

`api.invRollback()` &rarr; `UxmInvRollbackQuery`

| Method | Answers |
|---|---|
| `of(playerId)` | every snapshot held for that player, newest first |

The set is already bounded by the retention rules, per player and by age, so there is no limit to pass: what you get
is what the server kept. The items are not in it. A snapshot's contents are serialized Bukkit item stacks and there
is no honest way to hand them across this boundary; what is published is enough to list them and to name one for a
restore.

## Security

`api.security()` &rarr; `UxmSecurityQuery`

| Method | Answers |
|---|---|
| `of(playerId)` | which factors are on file, when they enrolled, and whether they are locked out |
| `isLockedOut(playerId)` | whether the account is inside a lockout window right now |

Enough for a staff panel to show who has enrolled and to explain why somebody cannot get in. Never the factor
itself: no PIN, no authenticator secret, no recovery material. Those exist so that only the account holder can
present them, and an API that handed them out would be handing out the account.

An account with no registration is an answer rather than an absence: nothing enrolled, no enrolment date. The
lockout is in-memory per-run state, so it is answered on the calling thread; a lockout the operator chose to write
to the ban list is a ban as well and reads as one there.

## Vote

`api.vote()` &rarr; `UxmVoteQuery`

| Method | Answers |
|---|---|
| `totals(playerId)` | their vote counts and streaks |
| `top(period, limit)` | the top voters for a period, ranked |
| `party()` | how close the server is to its next vote party |
| `queuedRewards(playerId)` | rewards waiting to pay out on their next join |

The party threshold is the effective one. An operator can raise the bar for the current round, and `/voteparty`
shows the raised figure, so a progress bar built from `count()` and `threshold()` matches what players are told.

## Messaging

`api.messaging()` &rarr; `UxmMessagingQuery`

| Method | Answers | Waits |
|---|---|---|
| `mailbox(playerId)` | their mail, newest first | yes |
| `unreadMail(playerId)` | how much unread mail they hold | yes |
| `ignoreList(playerId)` | who they are ignoring, and how much of each one's traffic | yes |
| `ignores(ownerId, otherId)` | whether the owner is ignoring the other player | yes |
| `acceptsMessages(playerId)` | their `/msgtoggle` switch | no |
| `isSocialSpying(playerId)` | their `/socialspy` switch | no |

Reading a mailbox does not mark anything read: that is the recipient's to do, and a consumer that displayed
somebody's mail and quietly cleared their unread count would take it from them.

An ignore is one-way. Alice ignoring Bob says nothing about what Bob hears.

## NPCs

`api.npc()` &rarr; `UxmNpcQuery`

| Method | Answers |
|---|---|
| `list()` | every NPC on the server |
| `get(name)` | one NPC, or empty |
| `exists(name)` | whether the name is taken, which is the check `/npc create` makes |
| `ownedBy(playerId)` | the NPCs that player created, which is what their limit counts |

What comes back is the shape of an NPC: its name, where it stands, what it renders as, its label, what a click
runs, whether it glows, who owns it. Not the render detail. An NPC carries several dozen appearance knobs that only
the renderer interprets, and publishing them would freeze that renderer's vocabulary into a promise we would then
have to keep. The skin is the clearest case: `skinned()` says whether one is set, and the base64 texture stays
where it is useful.

The label has three states, not two, and they are three different answers here. `displayName()` present is a label
that was set. `nameHidden()` true means it was explicitly cleared, so nothing shows above the NPC at all. Neither
means the NPC shows its own id, which is the default.

## Holograms

`api.holograms()` &rarr; `UxmHologramsQuery`

| Method | Answers |
|---|---|
| `list()` | every hologram on the server |
| `get(name)` | one hologram, or empty |
| `exists(name)` | whether the name is taken |

Text comes back as stored, before MiniMessage and before placeholders. There is no single rendered answer to give:
a line carrying `%player_name%` reads differently for every viewer, and the source is the only version that is the
same for everybody asking. A hologram that is not made of text carries its item, block, head texture or entity type
in `content()` instead, with `type()` saying which.

## Staff

`api.staff()` &rarr; `UxmStaffQuery`

| Method | Answers |
|---|---|
| `isInStaffMode(playerId)` | whether they are on duty right now |
| `modeOf(playerId)` | which named mode they are on, or empty |
| `inStaffMode()` | everybody on duty |

Nothing here waits: the state is a small in-memory map of the players who are online.

This is not the same question as "do they hold a staff permission". Somebody with every node in the plugin is off
duty until they turn it on, and a chat plugin marking a message as staff wants the toggle rather than the node.
Staff mode does not survive a quit, by design, so nobody comes back holding a gadget hotbar.

## Powertools

`api.itemworld()` &rarr; `UxmItemworldQuery`

| Method | Answers |
|---|---|
| `powertoolInHand(playerId)` | what the item in their main hand is bound to run, or empty |
| `powertools(playerId)` | every bound item in their inventory, in slot order |

The one readable corner of an otherwise stateless module. Repairing an item or aliasing the weather is a verb with
nothing behind it to read; a `/powertool` binding is state a player set, and a command-handling plugin has a real
reason to know about it before deciding what a click meant.

The binding lives on the item, not on the player, so it travels with the item: dropping it, trading it or putting
it in a chest takes the binding along. Both reads reach into a live inventory, so an offline player answers empty.


## Command gate

`api.commandControl()` &rarr; `UxmCommandControlQuery`

| Method | Answers |
|---|---|
| `check(playerId, command)` | what the gate would do with that command, and why, or empty when they are offline |
| `isBlocked(playerId, command)` | the same answer with everything but the yes or no dropped |

For anything that shows a player what they can run: a help menu that hides what is blocked, a GUI that greys out a
button, a companion plugin deciding whether to offer a shortcut. Asking here means agreeing with the gate rather
than reimplementing it, which matters because the rules are per group and per world and the two interact.

The command may be given with or without its leading slash and with or without its arguments; only the root is read,
exactly as the gate reads it. A namespaced form such as `minecraft:gamemode` is answered about the bare command when
the module is set to close that bypass, and about the namespaced root otherwise, again matching the gate.

`UxmCommandCheck` carries the answer and the reason for it:

| Field | Meaning |
|---|---|
| `command()` | the root the answer is about, lowercase and without its slash |
| `allowed()` / `blocked()` | whether it would run |
| `rule()` | `BYPASS`, `WHITELISTED`, `NOT_WHITELISTED`, `BLACKLISTED` or `NOT_BLACKLISTED` |
| `group()` | the permission group whose own list decided, or empty when the `default` list did |
| `world()` | the world whose per-world override decided, or empty when the server-wide rules did |

Both answers need the live player: the rules that apply depend on the world they are standing in and the
permissions they hold, and neither is knowable for somebody who is away. An offline player is an empty answer.

## Sidebar

`api.scoreboard()` &rarr; `UxmScoreboardQuery`

| Method | Answers |
|---|---|
| `hidden(playerId)` | whether they have put their sidebar away, or empty when they are offline |

The same durable preference `/scoreboard` flips and the render loop consults, so a panel showing a "sidebar: on /
off" control agrees with what the player actually sees. The preference survives a relog, but it is stored on the
player and only readable while they are here, which is why somebody who is away is an empty answer rather than a
default.


## A worked example

A `/profile` command in your own plugin, reading four surfaces, blocking on none of them, and behaving on a server
where half of them are switched off:

```java
public void showProfile(Player viewer, UUID subject) {
    UxmEssentialsApi api = UxmEssentialsApi.get();
    if (api == null) {
        viewer.sendMessage("uxmEssentials is not running");
        return;
    }

    // Straight away: these are in memory.
    api.presence().ifPresent(presence -> {
        if (presence.isAfk(subject)) {
            viewer.sendMessage("They are away right now");
        }
    });

    // Waits on a read; each continuation hops back before touching the player.
    api.playtime().ifPresent(playtime -> playtime.of(subject)
            .thenAccept(played -> send(viewer, "Playtime: " + played.totalConnected().toHours() + "h")));

    api.economy().ifPresent(economy -> economy.balance(subject)
            .thenAccept(money -> send(viewer, "Balance: " + money.amount() + " " + money.currency())));

    api.moderation().ifPresent(moderation -> moderation.ban(subject)
            .thenAccept(ban -> ban.ifPresent(sanction -> send(viewer, "Banned: " + sanction.reason()))));
}

private void send(Player viewer, String line) {
    viewer.getScheduler().run(this, task -> viewer.sendMessage(line), null);
}
```

The full sample plugin, including this idiom, is in
[`sample-consumer`](https://github.com/UXPLIMA/uxmEssentials/tree/main/sample-consumer) in the repository.
