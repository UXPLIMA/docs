---
title: Developer API
order: 395
description: Starting builds, reading jobs, the player market, adding a protection plugin, and listening to events.
icon: code
---

The public API is a separate artifact, `com.uxplima.builders:bukkit-api`. It holds the entry point, the
annotation for event handlers and the protection interface, and pulls in `uxmbuilders-app` (the facades,
command objects and views) and `uxmbuilders-domain` (the model, the exceptions and the events). The
current version is `2.0.0`.

## Adding it

<Tabs>
<Tab label="Gradle">

```groovy
repositories {
    maven { url = 'https://repo.uxplima.com/repository/maven-releases/' }
}

dependencies {
    compileOnly 'com.uxplima.builders:bukkit-api:2.0.0'
}
```

</Tab>
<Tab label="Maven">

```xml
<repositories>
    <repository>
        <id>uxplima-releases</id>
        <url>https://repo.uxplima.com/repository/maven-releases/</url>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.uxplima.builders</groupId>
        <artifactId>bukkit-api</artifactId>
        <version>2.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

</Tab>
</Tabs>

In your `plugin.yml`:

```yaml
softdepend: [ uxmBuilders ]
```

`compileOnly`: uxmBuilders is already on the server, and shading the API gives you a second copy that the
running plugin does not know about.

<Callout type="info" title="Moving from 1.0.0">

The 1.0.0 artifact `com.uxplima:builders-api`, the `UxmBuildersAPI` service, `BuildRequest` and the
Bukkit events are gone. Listeners register with uxmBuilders, not with Bukkit's `PluginManager`.

</Callout>

## The entry point

```java
import com.uxplima.builders.bukkit.api.UxmBuildersBukkitAPI;

if (!UxmBuildersBukkitAPI.isAvailable()) return;   // uxmBuilders is not installed or did not start
UxmBuildersBukkitAPI api = UxmBuildersBukkitAPI.getInstance();
```

`getInstance()` throws `IllegalStateException` while uxmBuilders is not enabled.

| Method | Gives |
|---|---|
| `buildFacade()` | Builds and the shop |
| `crewFacade()` | Builders and players' crews |
| `playerFacade()` | Trust, warehouses and build history |
| `marketFacade()` | The [player market](features/market/index.md): listings, selling, buying, reports and payouts |
| `registerEvent(Object)`, `unregisterEvent(Object)` | Event listeners, see below |
| `registerProtectionProvider(ProtectionProvider)`, `unregisterProtectionProvider(String id)` | Your own land checks |

## The facades

Every action comes in two forms: one that applies a player's checks and costs, and one that takes
`BuildCommandOptions`. Actions block while they check, charge and store: **call them from your own
worker thread, never from the main thread or a region thread**. Lookups are safe from any thread. A
refusal throws `BuildException`; `getStatus()` says why, `getStatus().messageKey()` is its key in
`messages.yml`, and `getDetails()` holds the values that message shows.

| Facade | Actions | Lookups |
|---|---|---|
| `BuildFacade` | `start`, `pause`, `resume`, `cancel`, `boost`, `purchase` | `activeJobs(UUID)`, `job(JobId)`, `allJobs()`, `packages()`, `findPackage(String)` |
| `CrewFacade` | `select`, `unlock`, `upgrade`, `setLevel` | `builders()`, `profile(UUID)`, `selected(UUID)`, `levelOf(UUID, String)` |
| `PlayerFacade` | `trust`, `untrust`, `deposit`, `withdraw` | `trusted(UUID)`, `warehouse(UUID)`, `history(UUID, page, pageSize)` (reads the database: call from a worker) |
| `MarketFacade` | See [The player market](#the-player-market) | |

Running jobs are returned as `JobView`, an immutable snapshot: id, owner, package, builder and level,
state, pause reason, placed and total blocks, speed, time left, boost, world, origin, bounds, queue
position and price paid.

| Options | Applies |
|---|---|
| `BuildCommandOptions.forPlayer()` | Everything: permissions, protection, placement rules, money, materials, cooldowns, limits, webhook, audit. The one-argument actions use this |
| `BuildCommandOptions.forAdmin()` | No permission, protection, rules, cost, materials, cooldown or limits; the webhook and the audit log still report it |
| `BuildCommandOptions.forSystem()` | The same as an admin, for actions the plugin takes on its own |
| `options.without(BuildSideEffect...)` | Any of the above with some checks removed |

## Starting a build

```java
Location spot = player.getLocation();
BuildStartCommand command = new BuildStartCommand(
        player.getUniqueId(),                       // owner: the build is theirs and they pay
        player.getUniqueId(),                       // actor: who asked for it
        "oak_house",                                // package id
        spot.getWorld().getName(),
        BukkitConverter.toBlockPos(spot),           // centre-bottom block of the build
        new Orientation(90, false),                 // rotation 0/90/180/270, mirrored or not
        0,                                          // what a blueprint already paid
        null);                                      // expected builder; null skips that check

Bukkit.getAsyncScheduler().runNow(plugin, task -> {
    try {
        BuildJob job = api.buildFacade().start(command);
    } catch (BuildException e) {
        player.sendMessage("Refused: " + e.getStatus().messageKey());
    }
});
```

A start runs every check a player's build does. With the player's checks the owner must be the actor and
online; a start for someone else is refused with `ERROR_OWNER_MISMATCH`. Pass
`BuildCommandOptions.forAdmin()` to start a free build for any player, ignoring protection and rules.
`BukkitConverter` turns `Location` and `Block` into `BlockPos` and `Cuboid` and back.

## The player market

`marketFacade()` reads the market and acts in it as a player would, with the same two forms of every
action, the same `BuildException` and the same rule: actions block, so call them from a worker.

| | Methods |
|---|---|
| Selling | `capture`, `create`, `updatePrice`, `updateDetails`, `pause`, `resume`, `delete`, `payout` |
| Buying | `purchase`, `issueBlueprint`, `report` |
| Lookups from memory, any thread | `enabled()`, `findListing(UUID)`, `findListing(ListingCode)`, `browse(MarketBrowseQuery)`, `listingsOf(UUID)`, `categories()`, `stats(UUID)` |
| Lookups that read the database | `purchasesOf(UUID)`, `earnings(UUID)`: call from a worker |

| Method | Does |
|---|---|
| `capture(MarketCaptureCommand)` | Reads the seller's selection into a `ListingDraft`, kept for ten minutes |
| `create(MarketListingCreateCommand)` | Lists a draft and charges the listing fee. The listing keeps the seller's commission rate of that moment: their `uxmbuilders.market.commission.<n>` while they are online on this server, `economy.commissionPercent` otherwise |
| `purchase(MarketListingPurchaseCommand)` | Buys one copy at `expectedPrice`, the price the buyer saw; a price changed since is refused. Returns a `MarketSale`: the licence, the listing after the sale, and whether the blueprint reached the inventory |
| `issueBlueprint(MarketBlueprintIssueCommand)` | A new blueprint for a purchase; every older copy stops building |
| `report(MarketListingReportCommand)` | Reports a listing, with the checks `/builders market report` applies |
| `payout(MarketPayoutCommand)` | Pays the seller what the market owes them. Returns a `MarketPayout`: what was paid, and what the seller still owes after refunds |
| `browse(MarketBrowseQuery)` | Listings on sale, by category, search and sort, a page at a time; with a viewer, only what that player may see |
| `stats(UUID)` | The figures the [placeholders](placeholders.md#market) show; empty for a player not online on this server |

```java
MarketFacade market = api.marketFacade();
Page<MarketListing> popular = market.browse(
        new MarketBrowseQuery(null, MarketSort.POPULAR, null, player.getUniqueId(), 0, 10));

MarketListing listing = market.findListing(ListingCode.parse("7KQ2MX")).orElseThrow();
Bukkit.getAsyncScheduler().runNow(plugin, task -> {
    try {
        MarketSale sale = market.purchase(new MarketListingPurchaseCommand(
                listing.getId(), player.getUniqueId(), player.getName(), listing.getPrice().toMajor()));
    } catch (BuildException e) {
        player.sendMessage("Refused: " + e.getStatus().messageKey());
    }
});
```

`BuildCommandOptions.forPlayer()` applies everything a player meets: permissions, the market ban,
cooldowns, limits, the listing fee and the price. With `forAdmin()` no money moves: a listing pays no
fee, and a purchase is a free grant that earns the seller nothing.

## A protection provider

```java
api.registerProtectionProvider(new ProtectionProvider() {
    public String id() { return "myclaims"; }
    public int priority() { return 60; }
    public boolean available() { return MyClaims.isEnabled(); }
    public Verdict canBuild(Player player, Cuboid area) {
        if (!MyClaims.hasClaimIn(area)) return Verdict.NOT_APPLICABLE;
        return MyClaims.canBuild(player, area) ? Verdict.ALLOWED : Verdict.denied("MyClaims", "claim name");
    }
});
```

| Method | Meaning |
|---|---|
| `id()` | The id in `protection.providers` and `/builders admin protection`; registering the same id again replaces the provider |
| `priority()` | Lower is asked first; 500 by default. The shipped ones use 10 to 50, and 900 for the event probe |
| `available()` | Checked on every query, so a plugin that disables later simply stops being asked |
| `canBuild(Player, Cuboid)` | `Verdict.ALLOWED`, `Verdict.denied(plugin, area)`, or `Verdict.NOT_APPLICABLE` where none of the box is your plugin's land |

Return `NOT_APPLICABLE` where your plugin has nothing in the box, so `PERMISSIVE` mode asks the others.
A provider that throws counts as a denial. `canBuild` may be called on any thread: a worker, the main
thread, or on Folia the thread of a region or player, so read your data in a thread-safe way and do not
touch blocks or chunks there. The `Cuboid` offers `samplePoints(n)` if your plugin answers point by
point. A server owner can still switch your provider off under `protection.providers.<id>`; a provider
the config does not name is on.

## Events

uxmBuilders has its own event bus. Its events live in `com.uxplima.builders.domain.event`, are not
Bukkit events, and are registered with `registerEvent`, not with `PluginManager`.

```java
import com.uxplima.builders.bukkit.api.annotation.BuildEventHandler;
import com.uxplima.builders.domain.event.BuildCompleteEvent;
import com.uxplima.builders.domain.event.BuildStartEvent;

public final class MyListener {

    @BuildEventHandler(priority = 5)
    public void onStart(BuildStartEvent event) {
        if (tooBusy()) {
            event.setCancelled(true);
            event.setCancelMessageKey("errorServerQueueFull");
        }
    }

    @BuildEventHandler(priority = 100, ignoreCancelled = true)
    public void onComplete(BuildCompleteEvent event) {
        // event.getJob() is a copy of the finished job
    }
}

// in onEnable
UxmBuildersBukkitAPI.getInstance().registerEvent(new MyListener());
```

| Attribute | Default | Meaning |
|---|---|---|
| `priority` | `1` | Lower runs first, the opposite of Bukkit's `EventPriority`; a handler that only watches the outcome should use a high value |
| `ignoreCancelled` | `false` | Skip this handler when an earlier one cancelled the event |

A handler is an instance method returning `void` with one event parameter; anything else is logged and
skipped. Handlers inherited from a superclass are registered too, and a handler for a base type
(`BuildEvent`, `MarketEvent`, `BuildersEvent`) receives every subtype. A handler that throws is logged and
the others still run.

| Event | Fires | Cancellable |
|---|---|---|
| `BuildPreValidateEvent` | A player is about to place a build, before any check | yes, with `setCancelMessageKey` |
| `BuildStartEvent` | The job is created, before anything is charged | yes, with `setCancelMessageKey` |
| `BuildProgressEvent` | Every 5%, from 5% | no |
| `BuildBlockPlaceEvent` | Every block, only with `api.fireBlockEvents`; carries the position and the block state placed | yes, skips the block and charges nothing for it |
| `BuildPauseEvent` | Before a pause; `isSafety()` for an offline owner, an unloaded world or missing materials | yes, except safety pauses |
| `BuildResumeEvent` | Before a paused job carries on | yes |
| `BuildBoostEvent` | Before a boost is paid; buyer, multiplier and end time; the price can change | yes |
| `BuildCancelEvent` | Before a cancel; the reason, and a refund that can change | yes, except safety cancels |
| `BuildCompleteEvent` | The last block is placed | no |
| `BuilderNpcSpawnEvent` | Before the NPC appears; name, skin texture and signature, and entity type can change | yes, the build runs without an NPC |
| `BuilderPurchaseEvent` | A player is about to unlock a builder; the price can change | yes |
| `BuilderUpgradeEvent` | A player is about to upgrade a builder; from and to level, and a price that can change | yes |
| `TrustAddEvent` | An owner is about to trust a player | yes |
| `TrustRemoveEvent` | An owner stopped trusting a player | no |

Events about a build extend `BuildEvent`, whose `getJob()` is a copy: changing it does nothing, the
event's own setters are the way to influence the outcome. Every event extends `BuildersEvent`.

Events are published on worker threads and on the thread that ticks the build (block, progress and
completion events), which is the main thread on Paper and the region's thread on Folia. Never assume the
main thread, keep handlers short, and do Bukkit work through the scheduler. Safety cancels (land lost,
server error, shutdown) and safety pauses go through even when a listener cancels them: a vetoed
`BuildCancelEvent` on lost land still cancels, with the refund your listener set.

### Market events

The player market's events are in `com.uxplima.builders.domain.event.market`, go through the same bus
and reach the same `@BuildEventHandler` methods. They extend `MarketEvent`; those about one listing extend
`MarketListingEvent` (`getListing()`), those about one purchase `MarketPurchaseEvent` (`getPurchase()`).
Both are copies taken after the change.

```java
@BuildEventHandler(priority = 5)
public void onList(MarketListingCreateEvent event) {
    if (event.getListing().getPrice().toMajor() > 1_000_000) event.setCancelled(true);
}

@BuildEventHandler(priority = 100, ignoreCancelled = true)
public void onSale(MarketListingPurchaseEvent event) {
    // event.getBuyer(), getPrice(), getFee() is the market's commission, getNet() the seller's share
}
```

| Event | Fires | Cancellable |
|---|---|---|
| `MarketListingCreateEvent` | A seller is about to list a design | yes |
| `MarketListingPriceChangeEvent` | A seller is about to change the price; the old and the new | yes |
| `MarketListingDetailsUpdateEvent` | A seller is about to change the title, description, category or icon; `setDetails` rewrites them, within the same limits | yes |
| `MarketListingDeleteEvent` | A seller is about to withdraw a listing for good | yes |
| `MarketListingPauseEvent`, `MarketListingResumeEvent` | A seller took a listing off sale, or put it back | no |
| `MarketListingPurchaseEvent` | A player is about to buy a copy; the buyer, the price, and how it splits into the market's fee and the seller's net | yes |
| `MarketListingSoldOutEvent` | The last limited copy sold | no |
| `MarketListingReportEvent` | A player is about to report a listing; the report | yes |
| `MarketListingHideEvent` | A listing went under review: reported often enough, or its seller was banned | no |
| `MarketListingRemoveEvent` | A moderator took a listing down; the moderator (null for the console) and the reason | no |
| `MarketListingRestoreEvent` | A moderator put a listing back | no |
| `MarketReportResolveEvent` | A moderator dismissed or acted on one report; `isActioned()` tells which | no |
| `MarketCreditConsumeEvent` | A build of a bought design started and used one of its builds | no |
| `MarketCreditReturnEvent` | A build went back to its purchase: the build could not start, or a [cancel gave it back](features/market/buying.md#cancelling-a-market-build) | no |
| `MarketBlueprintIssueEvent` | A blueprint was issued again; `getPrevious()` is the token the old copies carry | no |
| `MarketPurchaseRefundEvent` | A licence was taken back and the buyer refunded | no |
| `MarketPurchaseRevokeEvent` | A licence was taken back without a refund | no |

A cancelled event refuses the action with `EVENT_CANCELLED`, whose message is `errorRejectedByPlugin`.
The cancellable ones fire before the change is stored, so the action can still fail after them: a
purchase is refused when another server sold the last copy first, or when the economy will not take the
money. A removal closes the listing's open reports, and a restore dismisses them, without a
`MarketReportResolveEvent` for each.

## The domain types

| Type | What it is |
|---|---|
| `BuildJob` | A job: id, owner, package, builder and level, world, origin, orientation, state, cursor and total, progress, price paid, refund, boost |
| `BuildState` | `QUEUED`, `BUILDING`, `PAUSED`, `COMPLETED`, `CANCELLED` |
| `CancelReason` | `PLAYER`, `ADMIN`, `PROTECTION_LOST`, `SERVER_ERROR`, `MATERIALS_TIMEOUT`, `RECOVERY`, `SHUTDOWN` |
| `BuildPackage`, `BuilderType`, `BuilderLevel`, `CrewProfile` | The configured content and a player's crew |
| `JobId`, `BlockPos`, `Cuboid`, `Orientation`, `MaterialKey` | Value objects; a `Cuboid` is a world name and an inclusive box |
| `BuildException` | Every refusal, with a `Status` and the values its message shows |
| `MarketListing`, `MarketPurchase`, `ListingReport` | A listing, a buyer's licence and a report, in `com.uxplima.builders.domain.model.market` |
| `ListingCode`, `Money`, `ListingStatus`, `MarketSort` | A listing's code (`ListingCode.parse` reads one as players type it), an amount, a listing's status, a browse order |

Related: [Land protection](features/protection.md), [Integrations](integrations.md)
