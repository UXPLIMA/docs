---
title: Eggs and hatching
order: 3
description: Pools, weights, luck, multi-open, Robux eggs, platforms and the open animation.
icon: egg
---

## An egg

```lua
Pet1 = {
    order = 1,
    eggName = "Basic Egg",
    modelName = "Egg9",
    imageId = "rbxassetid://101387078158985",
    price = { currency = "Coins", amount = 2500 },
    multiOpen = { 1, 3, 8 },
    pool = { ... },
},
```

| Field | Meaning |
|---|---|
| `order` | Where it sits in the index list. No order sends it to the end |
| `eggName` | What players see |
| `modelName` | The `Model` in `Storage/Eggs` |
| `imageId` | `""` shows the 3D egg. An asset id shows a flat picture |
| `price` | A currency key and an amount |
| `robux` | `true` for a Robux egg |
| `devProductId` | The developer product id for a Robux egg |
| `multiOpen` | Which open-many buttons appear |
| `pool` | What comes out |

The key on the left, `Pet1`, is the id you put in an egg platform's `EggId` attribute.

## The pool

Each line is one possible outcome:

```lua
{ species = "Dragon", variant = "Rainbow", weight = 0.05, luckBoost = 1 },
```

| Field | Meaning |
|---|---|
| `species` | A key from `Config/Pets.luau` |
| `variant` | A key from `Config/Variants.luau` |
| `weight` | How common. Relative, not a percentage |
| `luckBoost` | Optional. Whether luck helps this entry |

Weights do not need to add to anything. A weight of 50 against a weight of 5 is ten times as
likely, and that is the whole rule.

The shipped Basic Egg totals 145.05, so its Rainbow Dragon at 0.05 is roughly one in 2,900.

## Luck

```lua
luckBoost = 1,
```

Luck raises an entry's effective weight:

```
effectiveWeight = weight * (1 + (luck - 1) * luckBoost)
```

| Luck | `luckBoost = 1` | `luckBoost` absent |
|---|---|---|
| 1 | Unchanged | Unchanged |
| 3 | Three times the weight | Unchanged |
| 5 | Five times the weight | Unchanged |

An entry without `luckBoost` is untouched by luck, so raising a player's luck shifts the
distribution toward the entries you marked, at the expense of everything else.

Put it on your rare entries only. Putting it on everything makes luck do nothing, because
every weight grows together.

Luck comes from the three luck gamepasses. See [Currencies and gamepasses](monetization.md).

## Multi-open

```lua
multiOpen = { 1, 3, 8 },
```

Which counts a player may open at once. The server rejects any count not in this list, so
the buttons and the rule are the same thing.

Each pet in a multi-open is rolled independently. Opening eight is eight separate rolls, not
one roll of a bigger prize.

## Robux eggs

```lua
robux = true,
devProductId = 1750131364,
price = { currency = "Coins", amount = 500 },
```

The price shown on the platform is fetched live from the product, so it always matches what
Roblox charges. The `price` block stays as a coin fallback.

<Callout type="danger" title="Replace the shipped product id">

`Pet2.devProductId = 1750131364` is somebody else's developer product. Players who buy it
pay them, and get nothing from you.

</Callout>

## Egg platforms

Found by the CollectionService tag **`EggPlatform`**, with a string attribute `EggId`.

| Instance | Purpose |
|---|---|
| The model, tagged `EggPlatform` | |
| `EggId` attribute | Which egg. Must match a key in `Config/Eggs.luau` |
| `ProximityPart` | Where the prompt sits. Any part is used if absent |
| `InfoPart.InfoText` | An icon and a price label, filled in for you |
| `Plate.Plane.PriceGui` | The same, on the platform surface |

A `ProximityPrompt` named `OpenPrompt` is created at runtime with no hold, custom style, no
line-of-sight requirement and an 8-stud range. It reads `Hatch` above the egg's name.

Platforms tagged after the server starts are set up immediately, so a placed or streamed-in
platform works.

<Callout type="info" title="A platform whose EggId matches nothing is skipped silently">

No prompt, no warning. If a platform does nothing, check the attribute spelling against the
config key first.

</Callout>

## What happens on open

| Step | |
|---|---|
| 1 | The profile must be loaded, or `NotLoaded` |
| 2 | The egg id must exist, or `BadEgg` |
| 3 | The count must be in `multiOpen`, or `BadCount` |
| 4 | The result must fit in the backpack, or `NoSpace` |
| 5 | At least `HatchCooldown` since the last open, or `Cooldown` |
| 6 | The price times the count is spent, or `Insufficient` |
| 7 | Each pet is rolled, added, counted and possibly announced |
| 8 | Auto-equip runs on each new pet |

The backpack check is done for the whole batch before anything is spent, so an eight-open
that would overflow is refused rather than half-completed.

Sending your pets to a breakable is cancelled when you open an egg, so they come back to
you for the reveal.

## Auto-delete

The client can pass a set of species and variant keys to discard on arrival. Those results
are still rolled and shown in the reveal, marked as auto-deleted, but never enter the
inventory.

That is what makes an eight-open of a low-tier egg tolerable once a player has a full
backpack.

## The open animation

`Config/EggAnim.luau`:

```lua
ShakeDuration = 1.1,
CrackDuration = 0.45,
HoldDuration = 0.15,
ShakeAmplitudeStart = 3,
ShakeAmplitudePeak = 22,
ShakeFrequencyStart = 7,
ShakeFrequencyPeak = 20,
PopScale = 1.3,
```

The egg shakes, building from gentle to strong, puffs up 30 percent, pops, holds for a
moment, and the pet appears.

The Fast Open gamepass skips it. Tune the durations down rather than off if you want a
quicker default: the shake is where the anticipation lives.

## Rare hatch announcements

```lua
Announce = {
    Enabled = true,
    Scope = "global",
    Variants = { Golden = true, Rainbow = true },
    Rarities = { Legendary = true },
    Format = "{player} hatched a {variant} {pet} (%{chance})!",
    HoldSeconds = 6,
},
```

A hatch is announced if **either** its variant or its rarity is listed. The shipped
configuration announces every Golden, every Rainbow and every Legendary.

| Token | Becomes |
|---|---|
| `{player}` | The player's display name |
| `{pet}` | The pet's display name |
| `{variant}` | The variant name |
| `{rarity}` | The rarity name |
| `{chance}` | The odds of that exact outcome, as a percentage |

Write the percent sign yourself; the token is the number alone. The shipped default also
begins with a celebration emoji, which you can remove.

`{chance}` is computed against **that player's luck**, so a lucky player's announcement
honestly reflects their better odds. Very small numbers are given more decimal places
automatically, so a one in three thousand hatch does not read as `0`.

| Scope | Reach |
|---|---|
| `"global"` | Every server in the game |
| `"server"` | Only the server it happened in |

<Callout type="tip" title="Announce sparingly">

On a busy game, announcing every Golden means a constant stream of chat. Start with just
the rarest variant and the top rarity, and widen only if the chat looks empty.

</Callout>
