---
title: Events
order: 1630
description: 'Everything uxmEssentials does is published as an ordinary Bukkit event,
  and the operations that can be refused cleanly are published twice: once as a question
  you may cancel, once as the fact afterwards.'
---

All of them live under `com.uxplima.uxmessentials.api.bukkit.event`, one package per context. You listen to them the
way you listen to anything else:

```java
@EventHandler
public void onHomeCreated(UxmHomeCreateEvent event) {
    getLogger().info(event.getPlayerName() + " set home " + event.getSlotNumber());
}
```

## The two kinds

| | Notification | Pre-event |
|---|---|---|
| Named | `Uxm<Thing><Action>Event` | `Uxm<Thing>Pre<Action>Event` |
| Says | this happened | this is about to happen |
| Cancellable | no | yes |
| Thread | the tick thread that owns the subject | whichever thread the operation is on |
| Bukkit API in the handler | yes | no |

Every context publishes notifications. Only nine operations have a pre-event, and that is deliberate: a veto point is
a promise that the operation can be refused with nothing half-done and nothing charged, and most of what a server
does cannot be taken back that way. A mail already read, a punishment already served, a warp already walked to.

## What you can cancel

| Event | Refusing it means | Asked |
|---|---|---|
| `UxmHomePreCreateEvent` | the home is not created | after the slot and the limit are checked, before the player is charged |
| `UxmHomePreDeleteEvent` | the home stays | before the row is removed |
| `UxmHomePreRelocateEvent` | the home stays where it is | before the move and before any fee |
| `UxmPlayerPreTeleportEvent` | the teleport does not happen | before the warmup starts, so the player does not stand still for nothing |
| `UxmWarpPreCreateEvent` | the warp is not created | before it is written |
| `UxmWarpPreDeleteEvent` | the warp stays | before it is removed |
| `UxmPlayerWarpPreCreateEvent` | the player warp is not created | after the owner's quota is checked, before anything is written |
| `UxmPlayerWarpPreDeleteEvent` | the player warp stays | only on the irreversible delete, never on archiving |
| `UxmKitPreClaimEvent` | the kit is not handed out | after the cooldown and permission pass, before the charge and before any item |

A cancelled operation fails cleanly. Nothing is written, nothing is charged, no cooldown is stamped, and the player is
told the action was blocked. It is not treated as an error, so nothing is logged as one. If you want them to know
why, tell them yourself.

`UxmPlayerPreTeleportEvent` is asked at the one point every voluntary teleport passes through, so a listener that
refuses teleports refuses `/home`, `/warp`, `/spawn`, `/back`, `/tpa` and `/rtp` alike, including whatever is added
later. Two things are deliberately not asked: an arrival after respawn or first join, since there would be nowhere to
leave the player, and a staff hard-delete of a player warp by id, since clearing an abusive warp must not be
blockable by whatever else happens to be installed.

```java
@EventHandler(ignoreCancelled = true)
public void onHomeCreating(UxmHomePreCreateEvent event) {
    // No Bukkit API here: this is not a tick thread.
    if (event.getLocation().world().endsWith("_nether")) {
        event.setCancelled(true);
    }
}
```

## Threading

**Notification events** are delivered on the tick thread that owns their subject: the player's region on Folia, the
main thread on Paper. Use the Bukkit API freely in those handlers.

**Pre-events** are different, and the difference is not cosmetic. The operation is blocked on your answer, so the
event fires on whichever thread the operation is on, which is usually a database thread. In those handlers:

- do not touch the Bukkit API, and
- keep it quick, because a player is waiting.

Read the event, decide, return. If you need to do something afterwards, schedule it.

<Callout type="note" title="Nothing costs anything when nobody is listening">

Both paths check for listeners before they build anything, so on a server with no plugin listening the whole
mechanism is a map lookup: about 7 ns, no allocation, nothing scheduled. You never need to worry that adding a
listener for one event makes the rest of the plugin slower.

</Callout>

## Shared shapes

Most events carry a subject player, and those extend a common base:

| Method | Gives you |
|---|---|
| `getPlayerId()` | the subject's UUID, always present, online or not |
| `getPlayerName()` | their name at the time of the event |
| `getOfflinePlayer()` | an `OfflinePlayer`, always |
| `getPlayer()` | the online `Player`, or `null` if they are not online (notification events only) |

Positions arrive as `UxmLocation`, a plain record of `world`, `x`, `y`, `z`, `yaw` and `pitch`. The world is its
name rather than a Bukkit handle, because an event can describe a place in a world that is not loaded; turn it into a
real location with `Bukkit.getWorld(loc.world())`, which answers `null` for an unloaded one. Money arrives as
`UxmMoney`, an amount and its currency id, because a server can run more than one currency.

## The catalogue

### Homes

| Event | Fires when | Carries |
|---|---|---|
| `UxmHomeCreateEvent` | a home was created | `getSlot()`, `getSlotNumber()`, `getLocation()` |
| `UxmHomeRelocateEvent` | a home was moved | slot |
| `UxmHomeDeleteEvent` | a home was deleted | slot |
| `UxmHomeRenameEvent` | a home was renamed | slot |
| `UxmHomeIconChangeEvent` | a home's icon changed | slot |
| `UxmHomeVisibilityChangeEvent` | a home was made public or private | slot |
| `UxmHomeLimitReachedEvent` | a player tried to set one home too many | `getCurrentCount()`, `getLimit()` |

### Teleport

| Event | Fires when | Carries |
|---|---|---|
| `UxmPlayerTeleportEvent` | a player was moved by uxmEssentials | `getKind()`, `getFrom()`, `getTo()` |
| `UxmWarmupStartEvent` | a warmup began | `getKind()`, `getOrigin()`, `getDuration()` |
| `UxmWarmupCancelEvent` | a warmup was cut short | `getKind()`, `getReason()` |
| `UxmBackLocationCaptureEvent` | a return point was recorded | `getLocation()`, `getCause()` |
| `UxmTeleportRequestSendEvent` | a `/tpa` or `/tpahere` was sent | `getDirection()`, `getExpiresAt()` |
| `UxmTeleportRequestAcceptEvent` | a request was accepted | request id, both players |
| `UxmTeleportRequestDenyEvent` | a request was denied | request id, both players |
| `UxmTeleportRequestCancelEvent` | the requester withdrew it | request id, both players |
| `UxmTeleportRequestExpireEvent` | a request timed out | request id, both players |

`getKind()` tells you which door the teleport came through: `REQUEST`, `BACK`, `RANDOM`, `SPAWN`, `HOME`, `WARP`,
`RESPAWN`, `ADMIN` or `POSITIONAL`.

### Warps and player warps

| Event | Fires when | Carries |
|---|---|---|
| `UxmWarpCreateEvent` | a server warp was created | `getWarpName()`, `getLocation()` |
| `UxmWarpDeleteEvent` | a server warp was deleted | warp name |
| `UxmPlayerWarpCreateEvent` | a player warp was created | `getWarpName()`, `getLocation()` |
| `UxmPlayerWarpDeleteEvent` | a player warp was deleted for good | warp name |

### Economy

| Event | Fires when | Carries |
|---|---|---|
| `UxmWalletCreditEvent` | money arrived | `getAmount()`, `getBalance()`, `getTransactionId()`, `getOccurredAt()` |
| `UxmWalletDebitEvent` | money left | `getAmount()`, `getBalance()`, `getTransactionId()`, `getOccurredAt()` |
| `UxmWalletRejectEvent` | a transaction was refused | `getRequested()`, `getAvailable()`, `getReason()` |
| `UxmBankDepositEvent` | a bank account was paid into | `getBankId()`, `getAmount()`, `getBankBalance()` |
| `UxmBankWithdrawEvent` | a bank account was drawn on | `getBankId()`, `getAmount()`, `getBankBalance()` |
| `UxmLoanDisburseEvent` | a loan was granted | `getLoanId()`, `getPrincipal()` |
| `UxmLoanRepayEvent` | a loan payment was made | `getLoanId()`, `getPaid()`, `getRemaining()` |

`getReason()` on a rejection is either `INSUFFICIENT_FUNDS` or `BALANCE_MAX_EXCEEDED`.

### Kits

| Event | Fires when | Carries |
|---|---|---|
| `UxmKitClaimEvent` | a kit was handed out | `getKitId()`, `getActorId()`, `isSelfClaimed()`, `getAt()` |

### Vaults

| Event | Fires when | Carries |
|---|---|---|
| `UxmVaultOpenEvent` | a vault was opened | `getIndex()`, `getViewerId()`, `isOwnVault()` |
| `UxmVaultContentsChangeEvent` | a vault's contents changed | `getIndex()` |

### Moderation

| Event | Fires when | Carries |
|---|---|---|
| `UxmPlayerWarnEvent` | a player was warned | `getIssuer()`, `getReason()`, `getExpiresAt()`, `getTotalWarnings()` |
| `UxmPlayerMuteEvent` | a player was muted | `getIssuer()`, `getReason()`, `getUntil()` |
| `UxmPlayerUnmuteEvent` | a mute was lifted | subject |
| `UxmPlayerJailEvent` | a player was jailed | `getJail()`, `getIssuer()`, `getReason()`, `getUntil()` |
| `UxmPlayerUnjailEvent` | a player was released | subject |
| `UxmPlayerTempbanEvent` | a player was temporarily banned | `getIssuer()`, `getReason()`, `getUntil()` |
| `UxmIpBanEvent` | an address was banned | `getIp()`, `getTarget()`, `getUntil()`, `getIssuer()` |
| `UxmAltDetectedEvent` | a join matched another account's address | `getIp()`, `getMatched()`, `isKicked()` |
| `UxmJailLocationDefineEvent` | a jail was defined | `getJail()` |
| `UxmJailLocationRemoveEvent` | a jail was removed | `getJail()` |

`getIssuer()` returns a `UxmIssuer`, which is a name plus an optional UUID, because the console issues punishments
too and it has no UUID.

### Player state

| Event | Fires when | Carries |
|---|---|---|
| `UxmPlayerHealEvent` | a player was healed | actor, `isSelfInflicted()` |
| `UxmPlayerFeedEvent` | a player was fed | actor, `isSelfInflicted()` |
| `UxmPlayerFlyToggleEvent` | flight was turned on or off | `isEnabled()` |
| `UxmPlayerGodToggleEvent` | god mode was turned on or off | `isEnabled()` |
| `UxmPlayerGameModeChangeEvent` | a game mode was set | `getMode()` |
| `UxmPlayerSpeedChangeEvent` | walk or fly speed was set | `getKind()`, `getScale()` |

### Messaging, presence and communication

| Event | Fires when | Carries |
|---|---|---|
| `UxmPrivateMessageEvent` | a `/msg` was delivered | `getRecipientId()`, `getMessage()`, `getSentAt()` |
| `UxmMailDeliverEvent` | mail was put in an inbox | `getSenderId()`, `getSenderName()`, `getMessage()` |
| `UxmHelpOpEvent` | a player raised a `/helpop` | `getMessage()`, `getRaisedAt()` |
| `UxmAfkEvent` | a player went AFK or came back | `isAfk()`, `isAutomatic()`, `getReason()` |
| `UxmBroadcastOptOutEvent` | a player opted in or out of announcements | `isOptedOut()` |
| `UxmAnnouncerReloadEvent` | the announcement list was reloaded | `getLineCount()` |

### Staff, scoreboard and poses

| Event | Fires when | Carries |
|---|---|---|
| `UxmStaffModeEvent` | staff mode was entered or left | `isEntered()` |
| `UxmStaffChatEvent` | a staff-chat message was sent | `getMessage()` |
| `UxmScoreboardVisibilityEvent` | a player hid or showed the scoreboard | `isHidden()` |
| `UxmPoseEvent` | a player sat, lay down or stood up | `isStarted()`, `getType()`, `getReturnLocation()`, `getTargetId()` |

### Worlds

| Event | Fires when | Carries |
|---|---|---|
| `UxmWorldCreateEvent` | a managed world was created | `getWorldName()` |
| `UxmWorldImportEvent` | an existing world folder was imported | world name |
| `UxmWorldAdoptEvent` | a world already on the server was adopted | world name |
| `UxmWorldLoadEvent` | a managed world was loaded | world name |
| `UxmWorldUnloadEvent` | a managed world was unloaded | world name |
| `UxmWorldDeleteEvent` | a managed world was deleted | world name |
| `UxmWorldUnregisterEvent` | a world stopped being managed | world name |
| `UxmWorldSettingChangeEvent` | a world setting was changed | `getSettingKey()`, `getSettingValue()` |
| `UxmWorldEntryDeniedEvent` | a player was refused entry | `getPlayerId()`, `getReason()` |

### Holograms and NPCs

| Event | Fires when | Carries |
|---|---|---|
| `UxmHologramCreateEvent` | a hologram was created | `getHologramName()`, `getLocation()` |
| `UxmHologramDeleteEvent` | a hologram was deleted | hologram name |
| `UxmNpcCreateEvent` | an NPC was created | `getNpcName()`, `getLocation()` |
| `UxmNpcMoveEvent` | an NPC was re-anchored | npc name, `getLocation()` |
| `UxmNpcDeleteEvent` | an NPC was deleted | npc name |

### Trade

| Event | Fires when | Carries |
|---|---|---|
| `UxmTradeCompleteEvent` | both sides confirmed and the swap settled | `getInitiatorItems()`, `getPartnerItems()`, `getInitiatorMoney()`, `getPartnerMoney()`, `getInitiatorExperience()`, `getPartnerExperience()` |
| `UxmTradeCancelEvent` | a trade ended without a swap | `getTradeId()`, both sides' ids and names |

Both name two players rather than one, so they extend `UxmEvent` and not the player event. A cancel covers a
cancel, a closed window and a disconnect alike: which of the three it was is not carried, because all three reach
the same path. The completion event fires whether or not the operator has the trade audit switched on.

### Ranks

| Event | Fires when | Carries |
|---|---|---|
| `UxmRankUpEvent` | a player moved up a rung | `getFromRank()`, `getToRank()` |
| `UxmRankSetEvent` | an administrator set a rank directly | `getPreviousRank()`, `getRank()` |
| `UxmPrestigeEvent` | a player prestiged | `getLevel()`, `getRewardMultiplier()` |

A rankup fires after the new rank is stored and its actions have run. `UxmRankSetEvent` may be about a player who
is offline, and `getPreviousRank()` is empty for one who had never been ranked before.

### Discord links

| Event | Fires when | Carries |
|---|---|---|
| `UxmAccountLinkEvent` | a code was redeemed and the accounts are bound | `getDiscordId()`, `getLinkedAt()` |
| `UxmAccountUnlinkEvent` | a binding was removed | `getDiscordId()` |

The code is redeemed on Discord's side, so the link event very often fires for a player who is not online. Do not
assume a live player from it. The unlink event carries the account that was bound because by the time it fires
there is nowhere left to look it up, and it covers all three ways a binding ends: the player, an operator, or a
plugin.

### Voting and item or world utilities

| Event | Fires when | Carries |
|---|---|---|
| `UxmVoteReceiveEvent` | a vote was credited | `getService()` |
| `UxmVotePartyEvent` | a vote party fired | `getThreshold()` |
| `UxmMobSpawnEvent` | staff spawned mobs | `getEntityType()`, `getRequested()`, `getSpawned()` |
| `UxmEntityPurgeEvent` | entities were cleared | `getScope()`, `getCategory()`, `getRadius()`, `getRemoved()` |

## Registering your listener

Nothing special is needed. There is no guard, no soft dependency, no load-order dance:

```java
@Override
public void onEnable() {
    getServer().getPluginManager().registerEvents(new MyListener(), this);
}
```

Your listener's signatures name uxmEssentials classes, which resolve from the API artifact you compiled against and
are present at runtime inside the plugin jar. If uxmEssentials is not installed, your listener simply never fires.

<Callout type="warning" title="A disabled module fires nothing">

Nine modules ship switched off. Silence from one of them means the operator turned it off, not that nothing
happened. Use `api.isModuleEnabled("homes")` when you need to tell the difference.

</Callout>

## Next steps

- [Adding the dependency](dependency.md)
- [Menu API](menu-api.md) to extend the GUI engine
- [The sample consumer](https://github.com/UXPLIMA/uxmEssentials/tree/main/sample-consumer), a compiling example
