---
title: Events
order: 182
description: Every API event, how to listen and cancel, which thread runs a handler, and the Bukkit mirrors.
icon: zap
---

uxmClaims fires its own events on its own event bus, in `com.uxplima.uxmclaims.api.event`. The same
events are also fired as Bukkit events through the generated mirrors in `uxmclaims-api-bukkit`, so a
plain `@EventHandler` listener works too.

## Two kinds of event

| Kind | Name | When | Can be cancelled |
|---|---|---|---|
| Before | `Pre*` (and `UpkeepCalculateEvent`) | Before a write, after its permission, limit and cost checks, on the thread that owns the write | Yes: `cancel()` or `cancel(String message)` |
| After | Everything else | After the database commit, on the thread that owns the action | No, it only reports |

Every event has `actor()`: an `Actor.Player`, `Actor.Console` or `Actor.System`. A cancelled write fails
with an `ActionException`; the text given to `cancel(String)` (MiniMessage) is its detail, which the
player sees in the module's "stopped by the server" message when the write came from a command or a menu.

## Listening on the event bus

```java
import com.uxplima.uxmclaims.api.UxmClaimsApi;
import com.uxplima.uxmclaims.api.event.EventPriority;
import com.uxplima.uxmclaims.api.event.land.LandEvent;
import com.uxplima.uxmclaims.api.event.land.PreLandCreateEvent;
import org.bukkit.plugin.java.JavaPlugin;

public final class SpawnGuardPlugin extends JavaPlugin {

    @Override
    public void onEnable() {
        UxmClaimsApi api = UxmClaimsApi.get();
        api.events().subscribe(this, PreLandCreateEvent.class, EventPriority.NORMAL, true, event -> {
            if (event.request().world().name().equals("spawn")) {
                event.cancel("<red>You cannot claim in the spawn world.");
            }
        });
        // A supertype gets every subtype: all land events.
        api.events().subscribe(this, LandEvent.class,
                event -> getLogger().info(event.getClass().getSimpleName() + " on " + event.land().name()));
    }
}
```

| Rule | Detail |
|---|---|
| Order | `LOWEST`, `LOW`, `NORMAL`, `HIGH`, `HIGHEST`, `MONITOR`; lowest runs first |
| `ignoreCancelled` | `true` skips the handler when an earlier one cancelled the event |
| `MONITOR` | Must only watch; a `MONITOR` handler that changes whether the event is cancelled is logged |
| Supertypes | A handler of `LandEvent`, `TownEvent`, `NationEvent`, `AreaEvent`, `WarpEvent` or `PlotEvent` gets every event of that group |
| Exceptions | Logged with your plugin's name; the write and the other handlers go on |
| Unsubscribing | `subscribe` returns a `Registration`; `close()` removes it. Everything is removed when your plugin disables |

A module subscribes through `context.events()` instead; see [Writing a module](sdk.md).

## Threads

| Event | Runs on |
|---|---|
| `Pre*` | The thread that owns the write: the main thread on Spigot and Paper, the acting player's thread (or the global thread) on Folia |
| After-commit events | The acting player's thread, or the global thread for the console and plugins. On Spigot and Paper both are the main thread |
| `LandEnterEvent`, `LandLeaveEvent`, `AreaEnterEvent`, `AreaLeaveEvent` | The moving player's thread, on every border crossing. Keep these handlers cheap |
| `UpkeepCalculateEvent` | The global thread |

On Folia, do world work for another place through that region's scheduler.

## Bukkit mirrors

Add `uxmclaims-api-bukkit` (see [Developer](index.md)) and listen the Bukkit way. Every concrete API
event has a mirror with the same simple name in `com.uxplima.uxmclaims.bukkit.event.<group>`, for example
`com.uxplima.uxmclaims.bukkit.event.land.PreLandCreateEvent`.

```java
import com.uxplima.uxmclaims.bukkit.event.land.PreLandCreateEvent;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;

public final class SpawnGuard implements Listener {

    @EventHandler(ignoreCancelled = true)
    public void onCreate(PreLandCreateEvent event) {
        if (event.getRequest().world().name().equals("spawn")) {
            event.setCancelled(true);
        }
    }
}
```

| Mirror rule | Detail |
|---|---|
| Getters | Each accessor in Bukkit style: `land()` becomes `getLand()`. `getEvent()` and `api()` return the API event |
| `getActor()`, `getPlayer()` | Who acted; `getPlayer()` is the acting player when online, else `null`. An event field named `player` is therefore `getPlayerId()` |
| When | After every handler on the uxmClaims bus has run, and only when a Bukkit listener is registered for that mirror class |
| Cancelling | `setCancelled(true)` on a `Pre*` mirror cancels the write. `setCancelled(false)` cannot undo a cancellation. The bus's `MONITOR` handlers ran before the mirror, so they do not see a cancellation made by a Bukkit listener; watch the after-commit event instead |
| Groups | Only concrete events are mirrored: there is no Bukkit `LandEvent` to listen to every land event |
| Async | The mirror is an async Bukkit event when it fires off the main thread (Folia region threads) |

The mirrors follow three settings in the kernel `config.yml` (see [kernel config](../configuration/kernel.md)):

| Key | Default | What it does |
|---|---|---|
| `api.bukkit-events.enabled` | `true` | Fires the Bukkit mirrors at all |
| `api.bukkit-events.pre-events` | `true` | Also mirrors the cancellable `Pre*` events |
| `api.bukkit-events.movement` | `true` | Also mirrors land and area enter and leave events |

The uxmClaims bus itself is never turned off by these. The [Skript](../modules/skript.md) add-on listens
to the mirrors, so its events stop with them.

## The events

Packages are under `com.uxplima.uxmclaims.api.event`. "Carries" lists the accessors besides `actor()`.

### land

| Event | Carries | Fired when |
|---|---|---|
| `PreLandCreateEvent` | `request()` (`CreateLand`) | A land is about to be created; not a `LandEvent`, there is no land yet |
| `LandCreateEvent` | `land()` | A land was created |
| `PreLandDeleteEvent` | `land()`, `reason()` | A land is about to be deleted |
| `LandDeleteEvent` | `land()` (last view), `reason()` | A land was deleted; `/uxm land restore` can still bring it back |
| `LandPurgeEvent` | `land()` (id), `name()` | A deleted land is gone for good |
| `LandRestoreEvent` | `land()` | A deleted land came back; follows a `LandCreateEvent` |
| `LandDeletedElsewhereEvent`, `LandRestoredElsewhereEvent` | `land()` | Network mode: another server deleted or restored a land, and this server read the change |
| `PreLandRenameEvent` / `LandRenameEvent` | `land()`, `newName()` / `oldName()` | A land is renamed |
| `PreLandChunkClaimEvent` / `LandChunkClaimEvent` | `land()`, `chunk()` | A chunk is added |
| `PreLandChunkUnclaimEvent` / `LandChunkUnclaimEvent` | `land()`, `chunk()` | A chunk is taken away |
| `PreLandFlagChangeEvent` / `LandFlagChangeEvent` | `land()`, `flag()`, `value()` | A flag changes |
| `PreLandMemberAddEvent` / `LandMemberAddEvent` | `land()`, `player()`, `roleId()` | A player becomes a member |
| `PreLandMemberRemoveEvent` / `LandMemberRemoveEvent` | `land()`, `player()` | A member is removed or leaves |
| `LandMemberRoleChangeEvent` | `land()`, `player()`, `roleId()` | A member got another role |
| `LandMemberOverrideChangeEvent` | `land()`, `player()`, `permission()`, `value()` | A member's own override changed; `UNSET` hands it back to the role |
| `PreLandOwnerTransferEvent` / `LandOwnerTransferEvent` | `land()`, `newOwner()` | A land changes owner |
| `LandSpawnChangeEvent` | `land()` | A land's spawn moved |
| `LandRoleCreateEvent`, `LandRoleDeleteEvent` | `land()`, `roleId()`, `name()` | A custom role was added or removed |
| `LandRoleRenameEvent` | `land()`, `roleId()`, `oldName()`, `newName()` | A role was renamed |
| `LandRolePriorityChangeEvent` | `land()`, `roleId()`, `oldPriority()`, `newPriority()` | A role moved; one move fires it for both swapped roles |
| `LandRolePermissionChangeEvent` | `land()`, `roleId()`, `permission()`, `value()` | A role permission changed |
| `LandEnterEvent`, `LandLeaveEvent` | `land()`, `player()` | A player walked into or out of a land |

### area, warp, claimblock, vault, expiry, invite, ban

| Event | Carries | Fired when |
|---|---|---|
| `PreAreaCreateEvent` / `AreaCreateEvent` | `land()`, `name()`, `bounds()` / `area()` | An area is created |
| `PreAreaDeleteEvent` / `AreaDeleteEvent` | `area()` / `area()`, `withLand()` | An area is deleted, on its own or with its land |
| `PreAreaRedefineEvent` / `AreaRedefineEvent` | `area()`, `bounds()` / `area()`, `oldBounds()` | An area is moved or resized |
| `AreaRenameEvent` | `area()`, `oldName()` | An area was renamed |
| `PreAreaFlagChangeEvent` / `AreaFlagChangeEvent` | `area()`, `flag()`, `value()` | An area flag changes |
| `PreAreaMemberAddEvent` / `AreaMemberAddEvent` | `area()`, `player()`, `roleId()` | A player becomes an area member |
| `AreaMemberRemoveEvent`, `AreaMemberRoleChangeEvent`, `AreaRolePermissionChangeEvent` | `area()` and the member or role | Area members and roles change |
| `AreaEnterEvent`, `AreaLeaveEvent` | `area()`, `player()` | A player walked into or out of an area |
| `PreWarpCreateEvent` / `WarpCreateEvent` | `land()`, `name()`, `position()` / `warp()` | A warp is created |
| `WarpDeleteEvent`, `WarpVisibilityEvent` | `warp()` | A warp was deleted (also with its land or chunk), or made public or private |
| `WarpRenameEvent`, `WarpRelocateEvent` | `warp()`, `oldName()` or `oldPosition()` | A warp was renamed or moved |
| `PreWarpTeleportEvent` / `WarpTeleportEvent` | `warp()` | A player starts the warm-up to a warp, or arrived |
| `ClaimBlockPlaceEvent`, `ClaimBlockDestroyEvent` | `land()`, `block()` | A claim block was placed or taken out of the world |
| `ClaimBlockChangeEvent` | `land()`, `block()`, `oldBlockKey()` | A claim block got another block type |
| `VaultOpenEvent` | `landId()`, `page()` | A player opened a vault page |
| `VaultUpdateEvent` | `landId()`, `page()`, `itemCount()` | A vault page was saved |
| `PreLandExpireEvent` | `land()`, `expiredAt()` | A land ran out of time; cancel to keep it (checked again later) |
| `LandRescheduleEvent` | `land()`, `before()`, `after()` | A land's expiry date changed |
| `PreLandInviteSendEvent` / `LandInviteSendEvent` | `land()`, `player()`, `roleId()` / `invite()` | An invite is sent |
| `LandInviteAcceptEvent` | `land()`, `invite()` | An invite was accepted; `land()` has the new member |
| `LandInviteRemoveEvent` | `land()`, `invite()`, `reason()` | An invite went away unaccepted |
| `PreLandBanEvent` / `LandBanEvent` | `land()`, `player()`, `reason()`, `expiresAt()` / `ban()` | A player is banned from a land |
| `LandUnbanEvent` | `land()`, `ban()` | A ban was lifted |

### town, nation, relation

| Event | Carries | Fired when |
|---|---|---|
| `PreTownCreateEvent` / `TownCreateEvent` | `request()` / `town()` | A town is founded with its home land |
| `PreTownDeleteEvent` / `TownDeleteEvent` | `town()` | A town is deleted |
| `PreTownRenameEvent` / `TownRenameEvent` | `town()`, `newName()` / `oldName()` | A town is renamed |
| `PreTownJoinEvent` / `TownJoinEvent` | `town()`, `player()` | A player becomes a resident |
| `PreTownLeaveEvent` / `TownLeaveEvent` | `town()`, `player()` | A resident leaves of their own accord |
| `PreTownKickEvent` / `TownKickEvent` | `town()`, `player()` | A resident is kicked |
| `PreTownMayorChangeEvent` / `TownMayorChangeEvent` | `town()`, `newMayor()` / `oldMayor()`, `newMayor()`, `succession()` | A town gets a new mayor; the `Pre` event is not fired for automatic succession |
| `PreNationCreateEvent` / `NationCreateEvent` | `name()`, `capital()` / `nation()` | A nation is founded |
| `PreNationDeleteEvent` / `NationDeleteEvent` | `nation()` | A nation is deleted |
| `PreNationRenameEvent` / `NationRenameEvent` | `nation()`, `newName()` / `oldName()` | A nation is renamed |
| `PreNationJoinEvent` / `NationJoinEvent` | `nation()`, `member()` | A town or land joins a nation |
| `PreNationLeaveEvent` / `NationLeaveEvent` | `nation()`, `member()`, `kicked()` | A town or land leaves or is removed |
| `PreNationCapitalChangeEvent` / `NationCapitalChangeEvent` | `nation()`, `newCapital()` / `oldCapital()`, `newCapital()` | A nation gets a new capital |
| `PreRelationChangeEvent` / `RelationChangeEvent` | `from()`, `to()`, `previous()`, `next()` | A relation changes, including a truce that ran out |
| `RelationRequestEvent` | `request()` | An alliance or truce was proposed |

### Economy, war and the rest

| Event | Carries | Fired when |
|---|---|---|
| `PreBankTransactionEvent` | `holder()`, `kind()`, `amount()` | Money is about to go into or out of a bank |
| `BankTransactionEvent` | `transaction()` | Any bank balance change, one per history line |
| `BankDepositEvent`, `BankWithdrawEvent` | `player()`, `transaction()` | A player paid in or took money out |
| `BankTransferEvent` | `out()`, `in()` | Money moved between two banks |
| `UpkeepCalculateEvent` | `holder()`, `day()`, `amount()`, `amount(BigDecimal)` | A holder's upkeep for the day was worked out; change it or cancel it |
| `TaxCollectEvent` | `day()`, `taxKind()`, `payerPlayer()`, `payer()`, `receiver()`, `amount()`, `paid()` | A tax or upkeep payment was settled |
| `NewDayEvent` | `day()`, `processed()` | An economy day was processed |
| `BankruptcyEnterEvent` / `BankruptcyLeaveEvent` | `holder()`, `debt()` / `holder()` | A holder went bankrupt, or paid its debt off |
| `PrePlotBuyEvent` | `plot()`, `player()`, `price()`, `isRent()` | A player is about to buy or rent a plot |
| `PlotBuyEvent`, `PlotRentStartEvent` | `plot()`, `buyer()` or `tenant()`, `price()` | A plot was bought, or a rental started |
| `PlotRentEndEvent` | `plot()`, `tenant()`, `reason()` | A rental ended: `LEFT`, `EXPIRED`, `UNPAID`, `EVICTED` or `REMOVED` |
| `PlotEvictEvent`, `PlotTypeChangeEvent` | `plot()`, `player()` or `previous()` | A tenant or owner was put out, or a plot got a new type |
| `PreLevelUpEvent` / `LevelChangeEvent` | `holder()`, `level()`, `cost()` / `holder()`, `previous()`, `level()`, `up()` | A town or nation reaches a level |
| `PreWarDeclareEvent` / `WarDeclareEvent` | `attacker()`, `defender()`, `mode()`, `goal()` / `war()` | A war is declared |
| `WarStartEvent`, `WarEndEvent` | `attacker()`, `defender()`, `war()`; `result()`, `reason()` on the end | The fighting starts, or the war ends |
| `WarPointCaptureEvent` | `war()`, `point()`, `captured()` | A capture point changed hands |
| `PreJailEvent` / `JailEvent` | `town()`, `player()`, `duration()`, `bail()` / `town()`, `sentence()` | A town jails a player |
| `UnjailEvent` | `sentence()`, `reason()` | A player left jail |
| `OutlawAddEvent`, `OutlawRemoveEvent` | `town()`, `player()` | A player became, or stopped being, an outlaw |
| `PreRuinEvent` / `RuinEvent` | `holder()`, `reason()` | A land or town falls into ruin |
| `RuinEndEvent` | `holder()`, `outcome()`, `player()` | A ruin was reclaimed, restored or deleted |
| `NewbieProtectionEndEvent` | `player()`, `reason()` | New-player protection ended: `expired`, `attacked`, `disabled` or `removed` |
| `RandomTeleportEvent` | `player()`, `target()` | The `rtp` module sent a player to a random spot |
| `GroupChatMessageEvent` | `channel()`, `group()`, `groupName()`, `sender()`, `senderName()`, `message()`, `source()` | A line was said in a town, nation or ally channel |

An event is fired only while its module is enabled.

Related: [The API](api.md), [Writing a module](sdk.md), [Webhooks](../modules/webhooks.md)
