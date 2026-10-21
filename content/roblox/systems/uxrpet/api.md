---
title: The server API
order: 9
description: Thirty server functions and eight signals for wiring pets into the rest of your game.
icon: code
---

`PetAPI` is a server-side module for your own scripts. Give a quest reward as a pet, gate a
door on a player's boost, or log every hatch.

```lua
local PetAPI = require(game.ServerScriptService.uxrPetSystem.Server.PetAPI)
```

Server only. Nothing here is reachable from a LocalScript.

## Reading pets

| Function | Returns |
|---|---|
| `GetPets(player)` | Every pet, as records |
| `GetEquipped(player)` | The equipped pets only |
| `CountPets(player)` | How many they own |
| `HasPet(player, species, variant)` | Whether they own one |

A record is:

```lua
{ uid = "...", species = "Dragon", variant = "Golden", locked = false, equipped = true, power = 250 }
```

## Changing pets

| Function | Does |
|---|---|
| `GivePet(player, species, variant, opts)` | Grants a pet. Returns its uid, or nil |
| `RemovePet(player, uid)` | Deletes it, ignoring the lock |
| `EquipPet(player, uid)` | Equips. Returns success and a reason |
| `UnequipPet(player, uid)` | Unequips |
| `LockPet(player, uid, locked)` | Sets the lock |
| `EquipBest(player)` | Unequips everything and equips the strongest that fit |

```lua
local uid = PetAPI:GivePet(player, "Dragon", "Golden", { equip = true, locked = true })
```

| Option | Effect |
|---|---|
| `equip` | Equip it immediately, if there is room |
| `locked` | Lock it so the player cannot delete or trade it |

`GivePet` returns `nil` if the profile has not loaded or the species or variant does not
exist. Check the return value before assuming the reward landed.

<Callout type="info" title="GivePet ignores the backpack cap">

Egg opens check the cap; this does not. A quest that grants a pet always succeeds, even to a
full inventory.

That is usually what you want for a reward. Check `CountPets` against `GetBackpackCap`
yourself if you would rather refuse.

</Callout>

`RemovePet` ignores the lock, unlike the player-facing delete. It is your script, so it is
assumed to know what it is doing.

## Power

| Function | Returns |
|---|---|
| `GetBoost(player)` | The equipped squad's combined power |
| `GetBoost(player, "all")` | Every owned pet's power, added |
| `GetBoost(player, "unequipped")` | The unequipped pets' power |
| `GetPetPower(species, variant)` | One pet's power, without owning it |

`GetBoost(player)` reads the `PetBoost` attribute, which is the same number that damages
breakables.

The `"all"` and `"unequipped"` scopes add raw values and do not apply `BoostMode` or resolve
mystery pets, so they are a rough measure of collection rather than an alternative squad
total.

## Currency

| Function | Does |
|---|---|
| `GetBalance(player, currency)` | Reads a balance |
| `AddBalance(player, amount, currency)` | Adds |
| `Spend(player, amount, currency)` | Spends, returning whether it succeeded |
| `Format(n)` | `1500` becomes `1.5K` |

The currency argument defaults to the main currency. These go through the same economy
adapter as everything else, so they respect a custom economy.

## Caps

| Function | Returns or does |
|---|---|
| `GetEquipCap(player)` | The full equip cap, bonuses included |
| `GetBackpackCap(player)` | The full backpack cap |
| `AddEquipSlots(player, n)` | Adds bonus equip slots |
| `AddBackpack(player, n)` | Adds bonus backpack space |
| `OwnsPass(player, keyOrId)` | Whether they own a pass, by config key or raw id |

`AddEquipSlots` and `AddBackpack` are how a quest, a level system or a shop of your own
grants slots without a gamepass. They stack on top of the base, the gamepasses and the index
goals.

<Callout type="warning" title="These bonuses do not persist">

They are stored as player attributes, `ApiEquipBonus` and `ApiBackpackBonus`, which are
gone when the player leaves.

Whatever earned the slots has to be saved by you, and the bonus reapplied on join. A player
who is granted two slots by a quest and rejoins has lost them unless your quest system grants
them again.

</Callout>

## Eggs and the index

| Function | Returns |
|---|---|
| `Hatch(player, eggId, count)` | Runs a normal open, with all its checks |
| `GetEggOdds(eggId, player)` | The pool with each entry's real chance for that player |
| `HasInIndex(player, species, variant)` | Whether it is collected |
| `GetIndexProgress(player)` | How many collected, out of how many exist |
| `GetCounts(player)` | Owned and equipped counts against their caps |
| `GetCatalog()` | Every pet, variant and egg definition |

`GetEggOdds` takes the player because luck changes the answer. Passing no player gives the
base odds.

## Signals

```lua
PetAPI.PetHatched:Connect(function(player, info)
    print(player.Name, "hatched", info.variant, info.species)
end)
```

| Signal | Fires with |
|---|---|
| `PetHatched` | `player, { species, variant, uid }` |
| `PetEquipped` | `player, uid` |
| `PetUnequipped` | `player, uid` |
| `PetDeleted` | `player, { uid, species, variant }` |
| `BalanceChanged` | `player, newBalance` |
| `BoostChanged` | `player, newBoost` |
| `IndexUnlocked` | `player, species, variant` |
| `BreakableBroken` | `player, { type, reward, position }` |

`Connect` returns a handle with a `Disconnect` method. Handlers run on their own threads, so
one that errors or yields does not hold up the game.

`IndexUnlocked` is the one to build achievements on: it fires only the first time a player
gets a given species and variant.

## Examples

Reward a quest with a pet and some coins:

```lua
local uid = PetAPI:GivePet(player, "Parrot", "Golden", { equip = true })
if uid then
    PetAPI:AddBalance(player, 5000)
end
```

Gate a door on squad power:

```lua
if PetAPI:GetBoost(player) >= 1000 then
    openDoor(player)
end
```

Grant slots for reaching a level:

```lua
levelSystem.LevelChanged:Connect(function(player, level)
    if level == 10 then
        PetAPI:AddEquipSlots(player, 2)
    end
end)
```

Log rare hatches:

```lua
PetAPI.PetHatched:Connect(function(player, info)
    if info.variant == "Rainbow" then
        myLogger:Post(player.Name .. " hatched a Rainbow " .. info.species)
    end
end)
```
