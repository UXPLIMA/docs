---
title: The API
order: 181
description: The entry point, the service of each module, writes with WriteOptions, views and futures.
icon: terminal
---

Everything starts at `UxmClaimsApi`. Reads come from memory and are safe on any thread; writes go
through the same checks and the same database pipeline as a player's command, and return a
`CompletableFuture`.

## Entry point

| Method | Returns |
|---|---|
| `UxmClaimsApi.get()` (static) | The API; `IllegalStateException` while uxmClaims is not enabled |
| `UxmClaimsApi.find()` (static) | `Optional<UxmClaimsApi>`, empty while uxmClaims is not enabled |
| `version()` | The running version, such as `4.0.0` |
| `lands()` | `LandService`; `IllegalStateException` when the `lands` module is not running |
| `protection()` | `ProtectionService`, the full permission chain |
| `profiles()` | `ProfileService`, every player uxmClaims has seen |
| `modules()` | `ModuleQuery`: `isEnabled(id)`, `isActive(id)`, `state(id)`, `ids()` |
| `service(Class<S>)` | A module's service, such as `TownService`; empty when that module is off |
| `events()` | The `EventBus`; see [Events](events.md) |
| `registries()` | Every registered flag and role permission, as `FlagInfo` and `RolePermissionInfo` |

```java
import com.uxplima.uxmclaims.api.UxmClaimsApi;
import com.uxplima.uxmclaims.api.key.PermissionKey;
import com.uxplima.uxmclaims.api.land.LandView;
import com.uxplima.uxmclaims.api.town.TownService;
import org.bukkit.entity.Player;

import java.util.Optional;

final class ClaimInfo {

    static void describe(Player player) {
        UxmClaimsApi api = UxmClaimsApi.get();
        Optional<LandView> land = api.lands().at(player.getLocation());
        boolean canOpen = api.protection().can(player, player.getLocation(),
                                               PermissionKey.parse("lands:container_open"));
        String town = api.service(TownService.class)
                .flatMap(towns -> towns.townOf(player.getUniqueId()))
                .map(t -> t.name())
                .orElse("none");
        player.sendMessage(land.map(LandView::name).orElse("Wilderness")
                           + " | chests: " + canOpen + " | town: " + town);
    }
}
```

## Protection

`ProtectionService` answers "may this player do this here" by walking the whole chain: admin bypass,
bans, owner, member overrides, the roles of the area, land, town and nation, relations, visitors and the
world defaults. Every method reads memory and is callable from any thread.

| Method | Answers |
|---|---|
| `canBuild(Player, Location)`, `canBreak(Player, Block)`, `canInteract(Player, Block)`, `canDamage(Player, Entity)` | The common checks |
| `can(Player, Location, PermissionKey)` | Any role permission, such as `warps:use`; tells the player when the answer is no |
| `canSilently(Player, Location, PermissionKey)` | The same, without telling the player |
| `canManage(Player or UUID, LandView, PermissionKey)` | Whether the player may manage the land with that key, without a place; the `UUID` form has no admin bypass |
| `landAt(Location)`, `isWilderness(Location)` | The land at a spot |

Keys are namespaced by the module that registers them: `FlagKey.parse("lands:pvp")`,
`PermissionKey.of("warps", "use")`. `registries()` lists the keys the enabled modules registered.

## Services by module

Get each with `api.service(Type.class)`. A module that is off has no service, so treat the
`Optional` as the normal case. The kernel provides `ProtectionService` and `ProfileService` itself;
every other service belongs to a module.

| Service (package `com.uxplima.uxmclaims.api.*`) | Module | What it holds |
|---|---|---|
| `land.LandService` | [lands](../modules/lands.md) | Lands, chunks, members, roles, flags; `at`, `byName`, `ownedBy`, `memberOf`, `contextual`, and every land write |
| `area.AreaService` | [areas](../modules/areas.md) | Block-precise boxes inside a land, with their own members, roles and flags |
| `expiry.ExpiryService` | [expiry](../modules/expiry.md) | A land's expiry: `info`, `price`, `extend`, `set`. `LandView.extension(ExpiryInfo.class)` is not wired to it, so read it here |
| `invite.InviteService` | [invites](../modules/invites.md) | Land invites: `send`, `accept`, `decline`, `revoke`, `invitesTo` |
| `ban.BanService` | [bans](../modules/bans.md) | Land bans with an optional reason and duration |
| `warp.WarpService` | [warps](../modules/warps.md) | Land warps: `ofLand`, `publicWarps`, `create`, `move`, `setPublic`. Writes run the same checks as `/claim warp` |
| `vault.VaultService` | [vault](../modules/vault.md) | Land vault contents: `items`, `itemCount`, `replace` |
| `claimblock.ClaimBlockService` | [claim-block](../modules/claim-block.md) | Claim blocks: `of`, `at`, `place`, `change`, `destroy` |
| `hologram.HologramService` | [holograms](../modules/holograms.md) | Floating text; not saved, so show it again after a restart |
| `border.BorderService` | [borders](../modules/borders.md) | Border particles: `show`, `hide`, `isShowing` |
| `economy.EconomyService` | [economy](../modules/economy.md) | Player money (Vault). Call it on the global thread |
| `claimpoints.ClaimPointsService` | [claim-points](../modules/claim-points.md) | Extra chunk allowance players earn or are given |
| `combat.CombatService` | [combat](../modules/combat.md) | Combat tags: `tag`, `untag`, `isTagged`, `remaining` |
| `newbie.NewbieProtectionService` | [newbie-protection](../modules/newbie-protection.md) | New-player PvP protection; offline players are not answered |
| `town.TownService` | [towns](../modules/towns.md) | Towns, residents, ranks: `townOf`, `owning(LandView)`, `can`, `join`, `kick`, `setMayor` |
| `nation.NationService` | [nations](../modules/nations.md) | Nations, members, capital, ranks |
| `relation.RelationService` | [relations](../modules/relations.md) | Diplomacy between lands, towns and nations: `between`, `effective`, `set` |
| `bank.BankService` | [bank](../modules/bank.md) | Treasuries kept in uxmClaims' database, not in Vault |
| `tax.TaxService` | [taxes](../modules/taxes.md) | Daily taxes, upkeep, debts and bankruptcy; reads only |
| `inbox.InboxService` | [inbox](../modules/inbox.md) | Notifications kept for offline players |
| `plot.PlotService` | [plots](../modules/plots.md) | Areas for sale or rent, plot types |
| `level.LevelService` | [levels](../modules/levels.md) | Town and nation levels, requirements and rewards |
| `war.WarService` | [war](../modules/war.md) | Wars, sides, goals, vassals |
| `jail.JailService` | [jails](../modules/jails.md) | Town jail cells, sentences, bail, outlaws |
| `ruin.RuinService` | [ruins](../modules/ruins.md) | Lands and towns in ruin, reclaiming and restoring |
| `rollback.RollbackService` | [rollback](../modules/rollback.md) | Chunk snapshots of lands and restoring them |
| `rtp.RandomTeleportService` | [rtp](../modules/rtp.md) | Random wilderness teleports |
| `productive.ProductiveLandService` | [productive-land](../modules/productive-land.md) | Crop growth, spawner rate and mob cap at a spot |
| `ranking.RankingService` | [leaderboards](../modules/leaderboards.md) | Leaderboards; register your own `RankingMetric` |
| `chat.GroupChatService` | [group-chat](../modules/group-chat.md) | Town and nation chat; `relay` carries lines in from outside the game |
| `land.OwnerNames` | kernel | Display name of any land owner (player, town, nation, server) |

Two service types are offered the other way round: you register them, uxmClaims calls them.
`region.RegionGuard` keeps claims out of another plugin's regions, and `map.MapLayerProvider` adds a
layer to the web maps. A module registers them with `services().register(Type.class, provider, 0)`.

## Writes

Every write takes a `WriteOptions`: who acts, and which checks to skip.

| Factory | Actor | Skips |
|---|---|---|
| `WriteOptions.asPlayer(UUID, String)` | `Actor.Player` | Nothing: permission, limit, cost, cooldown, region hooks, events and distance all apply |
| `WriteOptions.asAdmin(Actor)` | The one you pass | `PERMISSION`, `LIMIT`, `COST`, `COOLDOWN` |
| `WriteOptions.asSystem(String reason)` | `Actor.System(reason)` | `PERMISSION`, `LIMIT`, `COST`, `COOLDOWN` |
| `new WriteOptions(Actor, Set<Check>)` | The one you pass | Exactly the `Check`s you list |

`Check` has `PERMISSION`, `LIMIT`, `COST`, `COOLDOWN`, `REGION_HOOKS`, `EVENTS_CANCELLABLE` and `DISTANCE`.
`DISTANCE` (minimum distance between claims and towns) is skipped only when you list it. `Actor` is a
sealed interface: `Actor.Player(UUID id, String name, boolean admin)`, `Actor.Console()`, or
`Actor.System(String reason)`; events carry it as `actor()`.

## Futures and refusals

A write's future completes after the database commit, on the thread that owns the action: the main
thread on Spigot and Paper; on Folia the player's own thread, or the global thread for the console and
plugins. It fails with `ActionException` when a check,
a `Pre*` event or the database refuses; `messageKey()` names the language key, such as
`lands.error.not-enough-money`, and `module()` the module whose messages hold it.

```java
import com.uxplima.uxmclaims.api.UxmClaimsApi;
import com.uxplima.uxmclaims.api.action.ActionException;
import com.uxplima.uxmclaims.api.action.WriteOptions;
import com.uxplima.uxmclaims.api.land.LandView;
import org.bukkit.entity.Player;

import java.util.concurrent.CompletionException;

final class Renamer {

    static void rename(Player player, LandView land, String name) {
        UxmClaimsApi.get().lands()
                .rename(land.id(), name, WriteOptions.asPlayer(player.getUniqueId(), player.getName()))
                .whenComplete((renamed, error) -> {
                    if (error == null) {
                        player.sendMessage("Your land is now called " + renamed.name());
                        return;
                    }
                    Throwable cause = error instanceof CompletionException ? error.getCause() : error;
                    if (cause instanceof ActionException refused) {
                        player.sendMessage("Refused: " + refused.messageKey());
                    }
                });
    }
}
```

Do not block on the future (`join()`, `get()`) on the main thread: the write needs that thread to finish.

## Views

A read returns an immutable view of one moment: `LandView`, `MemberView`, `RoleView`, `TownView`,
`NationView`, `AreaView`, `WarpView`, `PlotView`, `WarView`, `ProfileView` and the rest. A write never
changes a view you hold; it returns a new one, and `revision()` grows by one on every write. Views are
safe to keep and to read from any thread. Only uxmClaims implements them.

| Type (package `api.model`) | Holds |
|---|---|
| `OwnerRef(OwnerType, UUID)` | Who owns a land: `PLAYER`, `TOWN`, `NATION` or `SERVER`. `OwnerRef.player(id)`, `OwnerRef.server()` |
| `HolderRef(HolderType, UUID)` | A land, area, town or nation that holds roles, money or relations. `HolderRef.area(id)`, `HolderRef.town(id)`, `HolderRef.nation(id)` |
| `WorldRef(UUID id, String name)` | A world, by id and name. `WorldRef.of(world)` from a loaded Bukkit world |
| `ChunkPos(int x, int z)`, `BlockPos`, `Position` | Coordinates; `ChunkPos.ofBlock(x, z)` from block coordinates |
| `Tri` | `ALLOW`, `DENY` or `UNSET` for flags, role permissions and overrides |
| `LandKind` | `PERSONAL`, `TOWN_HOME`, `OUTPOST`, `ADMIN` |

`LandView.extension(Class)` returns data another module adds to lands; it is empty when that module is off.

Related: [Events](events.md), [Writing a module](sdk.md), [Permissions](../permissions.md)
