---
title: Action API
order: 1650
description: 'Queries answer what is true. Actions change it: set a home, pay a player, hand
  over a kit, ban somebody, each running the same use case the matching command runs.'
---

Queries answer what is true. Actions change it: set a home, pay a player, hand over a kit, ban somebody. Every one
of them runs the same use case the matching command runs, so what your plugin does and what a player types are the
same operation, audited the same way and seen by the same listeners.

## Getting the surface

The write surface is handed out per plugin, and you pass yours in:

```java
UxmEssentialsApi api = UxmEssentialsApi.get();
if (api == null) {
    return;
}

UxmActions actions = api.actions(this);
actions.economy().ifPresent(economy ->
    economy.deposit(player.getUniqueId(), new BigDecimal("50"))
        .thenAccept(result -> result.ifFailed(failure ->
            getLogger().warning("could not pay the reward: " + failure.message()))));
```

There is no anonymous form on purpose. Your plugin's name is what the audit log records, what `/baltop`'s money
trail attributes the deposit to and what `/history` names as the issuer of a ban. A write nobody can trace back is
a write nobody can undo.

Each accessor is an `Optional` for the same reason the queries are: empty means the module is switched off, which is
a different thing from the operation failing.

## What comes back

Two shapes. Both are values, not exceptions.

| Type | For | Read it with |
|---|---|---|
| `UxmOutcome` | a write with nothing to report back | `succeeded()`, `ifFailed(...)`, `failureOrThrow()` |
| `UxmResult<T>` | a write that produces something (the home it created, the ban it laid down) | `succeeded()`, `value()`, `valueOrThrow()`, `failure()` |

```java
actions.homes().ifPresent(homes ->
    homes.set(playerId, 0, new UxmLocation("world", 100.5, 64, -200.5))
        .thenAccept(result -> {
            if (result.succeeded()) {
                getLogger().info("home set at slot " + result.valueOrThrow().slotNumber());
            } else {
                getLogger().info("refused: " + result.failureOrThrow().code());
            }
        }));
```

A `UxmFailure` carries a `code()` you can branch on and a `message()` for your own log. The codes are constants on
`UxmFailure`, and `failure.is(UxmFailure.NOT_FOUND)` reads better than a string comparison:

| Code | Means |
|---|---|
| `cancelled` | another plugin vetoed it through a pre-event |
| `not-found` | there is nothing by that name, slot or id |
| `already-exists` | there is already something there, and this verb will not overwrite it |
| `player-offline` | the write needed somebody at the keyboard and nobody was |
| `insufficient-funds` | the player cannot pay for it |
| `already-in-state` | it is already the way you asked for |
| `refused` | a rule said no: a limit, a permission, an exemption |
| `failed` | it broke rather than being denied |

An exception is a different thing from a failure, and the two arrive by different routes. A refusal the server
models is a failure value. A future that completes exceptionally means something broke. Invalid input, a negative
amount or a null id, throws straight away rather than being handed back as a failure.

## Threading

Every verb returns a `CompletableFuture`. Call them from anywhere, including a listener on the tick thread: the
write has already been moved to the right thread for you. **Never call `join()` or `get()` on the main thread.**

Where "the right thread" is depends on the write, and you do not have to know:

- most writes run on a worker thread, because they reach the database
- writes touching a live inventory, handing over a kit for instance, run on the thread that owns that player
- punishments run on the server's own thread, because banning somebody disconnects them and announces it

If your continuation touches the Bukkit API, hop back yourself, exactly as after any async work:

```java
economy.withdraw(playerId, cost).thenAccept(result ->
    getServer().getGlobalRegionScheduler().execute(this, () -> {
        if (result.succeeded()) {
            player.sendMessage("Paid.");
        }
    }));
```

---

## Economy

`actions.economy()` &rarr; `UxmEconomyActions`

| Method | Does |
|---|---|
| `deposit(playerId, amount)` | add to the balance, answering the new one |
| `withdraw(playerId, amount)` | take from it, refusing when they cannot afford it |
| `set(playerId, amount)` | write the balance exactly |
| `transfer(fromId, toId, amount)` | move money between two players |

Each has a second form taking a currency id for a server running more than one. An unknown currency is `not-found`,
and a negative amount throws.

`transfer` is one operation rather than a withdraw followed by a deposit, so there is no instant at which the money
exists nowhere. It answers with a `UxmOutcome`, since the interesting result is whether it happened.

```java
economy.transfer(buyerId, sellerId, price).thenAccept(outcome ->
    outcome.ifFailed(failure -> {
        if (failure.is(UxmFailure.INSUFFICIENT_FUNDS)) {
            // the buyer could not pay; nothing moved
        }
    }));
```

## Homes

`actions.homes()` &rarr; `UxmHomeActions`

| Method | Does |
|---|---|
| `set(ownerId, slot, location)` | create a home in an empty slot, answering it |
| `relocate(ownerId, slot, location)` | move an existing one |
| `rename(ownerId, slot, label)` | give it the name the player sees in the grid |
| `delete(ownerId, slot)` | remove it, and the invitations to it |

Slots count from zero and the slot is the identity. `set` on a slot that is taken is `already-exists` rather than a
silent overwrite; moving one is `relocate`. That is the same model the slot grid in `/home` shows, so your plugin
and the player are looking at one thing.

<Callout type="note" title="Nothing is charged">

A home cost is what the player pays for typing the command. A plugin setting a home on their behalf is not the
player, so no money moves either way. The player's home limit does still apply, because a home past it would be
one they cannot see: grant the limit node if a plugin should be able to exceed it.

</Callout>

The player is told what happened, in their own language, exactly as if they had run the command. An offline player
is told nothing and the write still lands.

## Warps

`actions.warps()` &rarr; `UxmWarpActions`

| Method | Does |
|---|---|
| `create(name, location)` | create a warp, answering it |
| `move(name, location)` | move an existing one |
| `delete(name)` | remove it |

The command's `/setwarp` both creates and moves. The API splits them, so a plugin creating a name that already
exists is refused with `already-exists` rather than quietly moving somebody else's warp.

The warp records your plugin as its owner, which is what `/warp info` then shows.

## Kits

`actions.kits()` &rarr; `UxmKitActions`

| Method | Does |
|---|---|
| `give(playerId, kitId)` | hand the items over with nothing in the way |
| `claim(playerId, kitId)` | run the player's own path, gates and all |

The two are genuinely different, and which you want depends on what you are building.

`give` is `/kit give`: no permission check, no cooldown started, no cost taken, and a one-time kit is still
unclaimed afterwards. It is what a reward, a crate or a quest should call.

`claim` is the player pressing the button: permission, cooldown, one-time claim state, cost, every gate the module
applies. A refusal says which one, `insufficient-funds` or `refused` with the reason in the message.

Both land on the recipient's own thread, and both need them online: `player-offline` rather than items dropped into
nowhere. A player who leaves between the call and the hop gets the same failure, so the future always completes.

## Moderation

`actions.moderation()` &rarr; `UxmModerationActions`

| Method | Does |
|---|---|
| `ban(targetId[, reason])` | ban until somebody lifts it |
| `tempBan(targetId, duration[, reason])` | ban for a span |
| `unban(targetId)` | lift a ban |
| `mute(targetId[, reason])` | mute until somebody lifts it |
| `tempMute(targetId, duration[, reason])` | mute for a span |
| `unmute(targetId)` | lift a mute |
| `kick(targetId[, reason])` | disconnect somebody |
| `warn(targetId, reason)` | record a warning, escalation rules included |
| `jail(targetId, jail[, duration], reason)` | jail in a named jail |
| `unjail(targetId)` | release |

A punishment handed down here is a punishment in every sense the server knows: the player is disconnected, the
sentence is stored, the history line is written, the operator audit records it, and any listener watching for one
hears it. The issuer is your plugin, which is what `/baninfo` and `/history` then show.

Durations are a `java.time.Duration`, and one that is zero or negative throws rather than laying down a punishment
that has already lapsed. A ban with no expiry comes back with `isPermanent()` true and `expiresAt()` empty.

Exemption is honoured: a target the operator made exempt is `refused` here exactly as they are for a staff member
typing the command. Rank duration caps are not, because those cap what a rank may hand down and a plugin holds no
rank.

Punishments announce themselves to the server. A plugin that punishes routinely and writes its own announcements
should work through `silently()`, so nobody is told twice:

```java
moderation.silently().tempBan(cheaterId, Duration.ofDays(7), "Flying")
    .thenAccept(result -> announceMyOwnWay(result));
```

The punishment still lands and is still audited; only the broadcast is left out.

## Player state

`actions.playerState()` &rarr; `UxmPlayerStateActions`

| Method | Does |
|---|---|
| `setGodMode(playerId, enabled)` | turn damage immunity on or off |
| `setFlying(playerId, enabled)` | turn flight on or off |
| `setGameMode(playerId, mode)` | put them in a game mode |
| `setWalkSpeed(playerId, multiplier)` | set walking speed; `0.2` is vanilla |
| `setFlySpeed(playerId, multiplier)` | set flying speed; `0.1` is vanilla |
| `heal(playerId)` | restore health and put out any fire |
| `feed(playerId)` | restore hunger and saturation |

These are setters, not toggles, which is the difference between an API and a keystroke: a plugin granting flight
for the length of an event wants flight on, and asking twice must not turn it off. Asking for a state a player is
already in succeeds and changes nothing.

The speeds are on the same scale `UxmPlayerState` reports, so what you write is what you read back. Out-of-range
values are clamped rather than refused.

All of it is state on a live player, so all of it needs one: an offline player is `player-offline`, and so is a
player who leaves between the call and the write.

## Presence

`actions.presence()` &rarr; `UxmPresenceActions`

| Method | Does |
|---|---|
| `setAfk(playerId, away)` | mark them away, or bring them back |
| `setAfk(playerId, reason)` | mark them away with a reason others see |

A setter again: marking somebody away who already is announces nothing a second time. Going away is announced and
the AFK list updates exactly as when the player types `/afk`.

## Vanish

`actions.vanish()` &rarr; `UxmVanishActions`

| Method | Does |
|---|---|
| `setVanished(playerId, vanished)` | hide the player, or show them again |

The whole of vanish, not a visibility trick: hidden from the tab list, from join and quit messages, from `/list`
and from the players a level below them, with the buffs their level grants applied and cleared alongside.

The level they are hidden at is not something to set. It is resolved from their own permission tier each time they
are hidden, so a plugin writing one would be writing a value the next resolve overwrites. Read where they ended up
with `api.vanish().levelOf(playerId)`.

## Teleport

`actions.teleport()` &rarr; `UxmTeleportActions`

| Method | Does |
|---|---|
| `teleport(playerId, location)` | send them somewhere, as `/tp` would |
| `back(playerId)` | return them to where they last were, as `/back` would |

`teleport` is the staff hop: no warmup to stand still for, no cooldown, no fee, nobody to accept it. Going through
it rather than through Bukkit is what buys the things that make a teleport safe here. The region hop happens off
the tick thread, passengers and vehicles come along, the arrival grace applies, the `/back` point is captured, and
the teleport event fires so everything watching stays in step.

Its future completes when the player has landed, not when the hop was asked for:

```java
teleport.teleport(playerId, arena).thenAccept(outcome -> {
    if (outcome.succeeded()) {
        startTheMatch();   // they are actually there
    }
});
```

`back` is the player's own return, gates and all: whether returning to a death point is allowed and how long they
must wait for it are the operator's settings, not the caller's. `not-found` when there is nowhere to go back to,
`refused` when there is but a rule says no. Its future completes once the return is accepted, since from there the
player owns it: a warmup they can walk out of is theirs to walk out of.

## Worlds

`actions.worlds()` &rarr; `UxmWorldsActions`

| Method | Does |
|---|---|
| `load(worldName)` | bring a registered world into the server |
| `unload(worldName)` | take it back out, saving it first |
| `unload(worldName, save)` | take it back out, saving it only if you say so |

The two operations `/world load` and `/world unload` run, over the same registry. A world has to be registered with
uxmEssentials before it can be loaded by name; creating one writes to disk on a scale an API call should not start
quietly, so it is not published here.

Both refuse rather than force. `not-found` for a world nobody registered or whose folder has gone,
`already-in-state` for one already loaded (or already unloaded), `refused` for the default world and for a world
with players still inside, `failed` when the disk itself said no.

The future completes when the world is in the state that was asked for, so loading and then teleporting somebody in
can be chained safely:

```java
worlds.load("event_arena")
    .thenCompose(outcome -> outcome.succeeded()
        ? teleport.teleport(playerId, new UxmLocation("event_arena", 0, 64, 0))
        : CompletableFuture.completedFuture(outcome));
```

Unloading without saving throws away everything that changed since the last save. That is what an arena rebuilt
every round wants and what nothing else does.

A world name that could leave the worlds folder is a malformed call and throws where you are standing.

## Vote

`actions.vote()` &rarr; `UxmVoteActions`

| Method | Does |
|---|---|
| `giveVote(playerId)` | credit one vote, as if a listing site had sent it |
| `giveVote(playerId, amount)` | credit several, one whole flow each |
| `addPartyProgress(votes)` | move the vote party counter along |

A credited vote is a real vote. It runs everything a vote arriving from a listing site runs: the streak, the
totals, the rewards, the broadcast, the party counter, and the party itself when the counter reaches its threshold.
That is the point of publishing it, and the reason to be careful with it: a hundred votes is a hundred votes' worth
of rewards.

The player does not have to be online. A vote for somebody who is away is queued the way a real one is and pays out
when they next join.

`addPartyProgress` is progress only: nobody is credited with a vote of their own and no streak moves. An amount
below one is a malformed call and throws.

## Messaging

`actions.messaging()` &rarr; `UxmMessagingActions`

| Method | Does |
|---|---|
| `sendMessage(senderId, recipientId, body)` | a private message, as `/msg` sends it |
| `sendMail(senderId, recipientId, body)` | mail from one player to another |
| `sendMail(recipientId, body)` | mail from your plugin |

A message goes through every gate a typed one goes through: a muted sender is refused, a recipient who has turned
messages off is refused, a recipient who ignores the sender is quietly dropped (the call still succeeds, so the
ignore stays unobservable), staff on socialspy see it, and both sides can `/reply` to it.

The sender has to be online, because the echo, the reply target and the socialspy line are all theirs. The
recipient does not: when they are offline and the server turns offline messages into mail, this leaves mail; when
it does not, this fails with `player-offline`. A vanished recipient takes that same offline branch, so the API
cannot be used to find out that somebody hidden is online.

Mail is the `/mail send` path: stored in the database, waiting however long it has to.

```java
messaging.sendMail(winnerId, "Your prize is waiting at spawn.");
```

Mail sent without a sender id comes from your plugin, under your plugin's name, the way a server notice does.
Nothing refuses it: there is no mute that applies to a plugin and no way for a player to ignore one.

A body over 256 characters, or blank, is a malformed call and throws. That is the width of the mail column, so a
body accepted here is a body that survives a restart.

## Next steps

- [Query API](queries.md) for reading what is true
- [Events](events.md) for hearing what happened, including what your own writes cause
