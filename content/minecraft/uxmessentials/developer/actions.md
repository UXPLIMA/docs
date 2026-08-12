---
title: Action API
order: 1650
description: 'Driving uxmEssentials from your plugin: homes, money, kits, punishments.'
---

Queries answer what is true. Actions change it: set a home, pay a player, hand over a kit, ban somebody,
each running the same use case the matching command runs.

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

## Ranks

`actions.ranks()` &rarr; `UxmRanksActions`

| Method | Does |
|---|---|
| `rankUp(playerId)` | promote them one rung, as `/rankup` does |
| `setRank(playerId, rankId)` | put them on a named rung, as `/setrank` does |
| `prestige(playerId)` | reset them to the first rung one prestige level higher |

`rankUp` and `prestige` are the player-facing verbs and run everything those verbs run: the requirements are
checked, the cost is charged, and the rank's own actions fire. Each refusal comes back as a code rather than an
exception: `refused` for an unmet requirement, `insufficient-funds` for a cost the wallet would not cover,
`already-in-state` for a player already on the top rung or at the prestige cap. Both need the player online,
because a requirement may read their inventory, and answer `player-offline` when they are not.

`setRank` is the administrative one. No requirements, no cost, and an offline account is fine, which is the case
it exists for. A rank id that is not on the ladder answers `not-found` rather than storing a pointer to nothing.

Prestige is a module switch. With `prestige.enabled` off the server publishes no `/prestige` verb, and this action
answers `refused` rather than pretending a switched-off mechanic ran.

## Discord links

`actions.discordLink()` &rarr; `UxmDiscordLinkActions`

| Method | Does |
|---|---|
| `unlink(playerId)` | remove their Discord binding, as `/discordunlink` does |

One verb, and deliberately. There is no force-link: a binding is only real once the player proved it in game with
a one-time code, and one written from outside would say something the player never agreed to. Removing one is
different, which is why a panel or a bot can do it.

It writes one row, so an offline account is fine, which is most of what this is called for. A player who had no
binding answers `not-found` rather than a quiet success, so a caller can tell a removal from a no-op.

## Inventory snapshots

`actions.invRollback()` &rarr; `UxmInvRollbackActions`

| Method | Does |
|---|---|
| `restore(playerId, snapshotId)` | put a stored inventory back, as `/invrestore` does |

The same safety copy the command takes is taken here: what the player is holding right now is frozen as its own
snapshot before it is overwritten, so a restore run by mistake can itself be undone.

The player has to be online. A snapshot is applied to a live inventory and never written to disk, so an offline
target answers `player-offline`; their snapshots keep, and the restore works once they return. A snapshot id that no
longer resolves, pruned or already restored, answers `not-found`.

There is no capture verb. Snapshots are taken by the events that make them worth taking, a death and a logout, and
one minted on request would sit in the same bounded set and push a real one out of it.

## Security

`actions.security()` &rarr; `UxmSecurityActions`

| Method | Does |
|---|---|
| `forceVerification(playerId)` | make the account prove its factor again on the next join |
| `clearLockout(playerId)` | end a lockout early |

Both go in the safe direction. Forcing a verification forgets every device the account is trusted from, which is
what a panel does when an account looks compromised; an account holding no factor has nothing to be made to prove
and answers `not-found`. Clearing a lockout is what staff do for a player who locked themselves out; an account that
is not locked out answers `already-in-state`, and a lockout the operator chose to write to the ban list is lifted by
the unban that lifts every other one.

There is deliberately no enrol and no reset. Enrolling for somebody else would mean minting a secret they never saw,
and clearing an account's factors is a security downgrade that belongs behind an operator's own command, where it is
logged as a person having done it.

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

## Vaults

`actions.vaults()` &rarr; `UxmVaultsActions`

| Method | Does |
|---|---|
| `open(ownerId, index)` | put the real vault window in front of its owner, as `/vault <n>` does |
| `delete(ownerId, index)` | remove a vault, and the items in it with it |
| `rename(ownerId, index, name)` | name a vault, which is what the selector shows |
| `clearName(ownerId, index)` | drop the name, so the selector falls back to the number |
| `setIcon(ownerId, index, material)` | draw the vault with this material in the selector |
| `clearIcon(ownerId, index)` | drop the icon, so the default is drawn again |

Vault numbers count from one, the way the owner types them.

There is nothing here to put items in a vault with, or take them out. An item stack is a Bukkit value with no
published form, and the window is where the item policy, the size quota and the save-on-close live: a write that
went around it would be a second, weaker way into the same storage. `open` hands the player the real window
instead.

These run the owner's own path rather than a staff override. Their amount quota gates a vault that does not exist
yet, a configured fee is charged to them and a configured refund is paid back, and they are told what happened in
their own language. A vault handed out around the quota would be one the plugin's own selector refuses to draw.

`open` needs them online, because a window has to be shown to somebody; the other five work whether they are here
or not. An icon that is not a real material, or a custom icon at all when the operator switched them off, answers
`refused`.

## Player warps

`actions.playerWarps()` &rarr; `UxmPlayerWarpsActions`

| Method | Does |
|---|---|
| `create(actorId, name, location)` | create a warp, or re-anchor their own one of that name |
| `relocate(actorId, name, location)` | move an existing warp |
| `rename(actorId, name, newName)` | rename one in place |
| `archive(actorId, name)` | retire one, recoverably |
| `restore(actorId, name)` | bring an archived one back |
| `delete(actorId, name)` | drop one for good, freeing its name |

Every verb names the player it acts as, because a warp's rules are written in terms of a person: the owner may
remove it, a manager may move it, a stranger may do neither. Passing somebody who is not entitled gets exactly the
answer they would get in game, `refused`, rather than a quiet success.

`archive` is the one a cleanup wants. It takes the warp out of the listings and stops travel to it while the warp
itself, its whitelist, its bans, its earnings and its history all survive, and `restore` puts it back. `delete` is
the irreversible one: the row goes and so does everything hanging off it.

Names are server-wide unique, lowercase, three to thirty-two characters of `a-z`, `0-9`, `_` and `-`. A name that
does not fit that shape answers `refused` rather than throwing, so a panel passing user input straight through gets
a message it can show. A world no loaded world answers to is `not-found`.

Nothing is charged and no rent is settled: those follow from a player running the command. The owner's warp limit
does still apply to a new warp.

## NPCs

`actions.npc()` &rarr; `UxmNpcActions`

| Method | Does |
|---|---|
| `create(actorId, name, location)` | put up an NPC standing there |
| `move(actorId, name, location)` | move it |
| `setSkin(actorId, name, skinOwner)` | dress it in that account's skin |
| `clearSkin(actorId, name)` | take the skin back off |
| `setDisplayName(actorId, name, label)` | show a label above it |
| `hideDisplayName(actorId, name)` | show nothing above it |
| `clearDisplayName(actorId, name)` | go back to showing its id |
| `setClickCommand(actorId, name, command)` | run a command when it is clicked |
| `clearClickCommand(actorId, name)` | stop a click running one |
| `delete(actorId, name)` | remove it |

Every verb names the player it acts as, and runs the use case `/npc` runs for them. So a create is charged to that
player's NPC limit and records them as the owner. Pass whoever asked for it; an NPC nobody owns is one an operator
cannot ask anybody about.

A skin is named by the account that wears it rather than by a base64 texture, and is resolved through the same
server-wide lookup `/npc skin` uses, which works on an offline-mode server too. A name no account answers to is
`not-found`.

The three label verbs are three states rather than two. `hideDisplayName` shows nothing at all; `clearDisplayName`
goes back to showing the NPC's id, which is the default.

An NPC moves inside the world it stands in. A location in another world is `refused` rather than half-honoured:
moving one across worlds means despawning it for everybody watching and spawning it for a different set, which is a
copy and a delete rather than a move.

What is not here is the render detail: equipment, pose, scale, per-type metadata. Those stay behind `/npc` because
their vocabulary is the renderer's, and publishing it would freeze it.

## Holograms

`actions.holograms()` &rarr; `UxmHologramsActions`

| Method | Does |
|---|---|
| `create(actorId, name, location, firstLine)` | put up a text hologram showing one line |
| `move(actorId, name, location)` | move it |
| `addLine(actorId, name, text)` | add a line at the bottom |
| `setLine(actorId, name, line, text)` | replace one line |
| `removeLine(actorId, name, line)` | remove one line |
| `setClickCommand(actorId, name, command)` | run a command when it is clicked |
| `clearClickCommand(actorId, name)` | stop a click running one |
| `delete(actorId, name)` | remove it |

A hologram put up this way is an ordinary hologram: same table, same rendering, and an operator can edit or delete
it with the commands afterwards rather than having to go back to whatever put it there.

Line numbers count from one, the way the commands number them and the way the hologram reads on screen. A number
past the end is `not-found`. Removing the last remaining line is `refused`: a hologram keeps at least one, and an
empty one would be an invisible thing an operator cannot find. Delete it instead.

Creating takes one line rather than a list, because a hologram must always have at least one and starting with
exactly that makes the rule impossible to trip over. Add the rest with `addLine`.

Text is stored as given, MiniMessage and placeholders included, both resolved per viewer at render time. A
hologram carrying a placeholder needs a refresh interval to keep up with it, which is set with `/hologram refresh`.

## Sidebar

`actions.scoreboard()` &rarr; `UxmScoreboardActions`

| Method | Does |
|---|---|
| `refresh(playerId)` | redraw their sidebar now rather than at the next refresh |
| `hide(playerId)` | put it away, as `/scoreboard` does |
| `show(playerId)` | bring it back |

The sidebar is redrawn on a timer, so anything that changes what it should say shows up within one refresh interval
without asking for anything here. `refresh` is for when that wait is too long: a rank change, a balance the player
just earned, a placeholder your own plugin owns and has just moved.

Hiding and showing write the same durable preference `/scoreboard` flips, so a player who is put away stays that way
across a relog until somebody brings it back. Both go through the same use case the command does, which means the
player is told what happened and the visibility event fires exactly as it would have. Asking for the state they are
already in is `already-in-state` rather than a silent flip to the opposite.

## Tab list

`actions.tablist()` &rarr; `UxmTablistActions`

| Method | Does |
|---|---|
| `refresh(playerId)` | repaint their tab list now rather than at the next refresh |

One verb, because there is only one honest one. The header, footer, list names and ordering are all authored in the
module's config and repainted on a timer; nothing outside the module owns a row it could set or take away, and
anything that cleared the list here would be repainted a tick later.

## Nametags

`actions.nametags()` &rarr; `UxmNametagActions`

| Method | Does |
|---|---|
| `refresh(playerId)` | re-select and redraw the nametag above their head |

Which format a player wears is decided by the module from their permissions, world and state, and a reconcile pass
re-applies that decision for everybody on a timer. `refresh` runs that pass for one player immediately, which is
what you want after giving somebody a rank or changing a placeholder the nametag reads. It re-selects as well as
redraws, so a player who should now wear a different format does; a player no format applies to any more has their
nametag removed, which is the correct outcome and still reported as success.

Taking a nametag away from outside is not offered, for the same reason the tab list cannot be cleared: it would last
until the next reconcile pass.


## Next steps

- [Query API](queries.md) for reading what is true
- [Events](events.md) for hearing what happened, including what your own writes cause
