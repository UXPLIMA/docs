---
title: Query API
order: 1640
description: 'Events tell you what uxmEssentials did. Queries answer what is true
  right now: how many homes a player owns, what they hold, whether they are banned,
  who is away, which worlds are loaded.'
---

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

---

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

<Callout type="tip" title="Filtering a list? Use `canSee`">

Vanish has levels: a player is hidden *at* a level, and a viewer sees them when their own level reaches it. On a
server that layers the tiers, `isVanished` alone would hide staff from each other. `canSee` is the question the
tab list, the join messages and the staff tools all ask, and asking it too keeps your list agreeing with theirs.

</Callout>

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

---

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
