---
title: Pets, rarities and variants
order: 2
description: Adding a pet, the six rarities, the variant multipliers, and mystery power.
icon: cat
---

## Adding a pet

One block in `Config/Pets.luau` and one `Model` in `Storage/Pets` with a matching name.

```lua
Dragon = {
    displayName = "Dragon",
    rarity = "Legendary",
    baseBoost = 50,
    flying = true,
    followOffset = Vector3.new(0, 4, -3),
    modelName = "Dragon",
    imageId = "",
},
```

| Field | Meaning |
|---|---|
| `displayName` | What players see |
| `rarity` | A key from `Config/Rarities.luau`. Sets the colour |
| `baseBoost` | Raw power before variant multipliers |
| `flying` | `true` floats, `false` walks |
| `followOffset` | Where it sits behind the player |
| `modelName` | The `Model` in `Storage/Pets` |
| `imageId` | `""` shows the 3D model. An asset id shows a flat picture instead |
| `mysteryPower` | Optional. See below |

The key on the left is the species id used everywhere else: in egg pools, in the index, in
the API. Renaming it after launch orphans every pet players already own.

## The shipped catalog

Fifteen pets, ranging from a `baseBoost` of 1 to 50:

| Rarity | Pets |
|---|---|
| Common | Cat, Dog, Lamp, Bunny, Chicken, Duck, Pig |
| Uncommon | Lion, Monkey, Cow, Bee |
| Rare | Parrot, Axolotl |
| Epic | Elephant |
| Legendary | Dragon |

Bunny is the one with an `imageId` set, as a working example of the flat-picture option.

## Follow offsets

```lua
followOffset = Vector3.new(2, 0, -2),    -- ground pets
followOffset = Vector3.new(0, 3, -2),    -- flyers
```

This is the pet's own offset within its slot in the trailing grid, not its position. Raise Y
for flyers so they hover above head height rather than clipping through the player.

The grid itself is tuned in [Following and power](following.md).

## Rarities

`Config/Rarities.luau` is presentation only. It maps a rarity name to a look:

```lua
Legendary = {
    backgroundImage = "rbxassetid://17336711281",
    color = Color3.fromRGB(243, 193, 10),
    gradient = "Gold",
},
```

| Field | Used for |
|---|---|
| `backgroundImage` | The coloured tile behind the pet in menus |
| `color` | The fallback text colour |
| `gradient` | A gradient name from `Config/Gradients.luau`, for the rarity text |

Six ship: Common, Uncommon, SemiRare, Rare, Epic, Legendary.

<Callout type="info" title="Rarity does nothing mechanically">

It sets the colour and it decides which hatches are announced. It does not affect odds:
odds come entirely from the weights in each egg's pool.

A Legendary pet in a pool with a weight of 50 is common. Keep the two in step deliberately.

</Callout>

`SemiRare` ships with a look but no pet using it. Add one, or leave it as a slot for later.

## Variants

A variant is a stronger and often different-looking version of the same species.

```lua
Golden = { boostMultiplier = 5, tag = "Golden", render = { modelPrefix = "Golden" } },
```

| Field | Meaning |
|---|---|
| `boostMultiplier` | Multiplies the species' `baseBoost` |
| `sizeMultiplier` | Scales the model in the world. Omit for normal size |
| `tag` | The word before the name, giving `Golden Cat` |
| `mysteryPower` | The power becomes `???`. See below |
| `render.modelPrefix` | Use a separate model named prefix plus species |

The six shipped variants:

| Variant | Multiplier | Size | Model | Power |
|---|---|---|---|---|
| `Normal` | 1 | 1 | The base model | Fixed |
| `Golden` | 5 | 1 | `Golden<Species>` | Fixed |
| `Rainbow` | 25 | 1 | `Rainbow<Species>` | Fixed |
| `Huge` | ignored | 1.5 | The base model | Mystery |
| `GoldenHuge` | ignored | 1.5 | `Golden<Species>` | Mystery |
| `RainbowHuge` | ignored | 1.5 | `Rainbow<Species>` | Mystery |

A `modelPrefix` of `"Golden"` on species `Cat` looks for a model named `GoldenCat` in
`Storage/Pets`. Leave it `""` to reuse the base model, which is what the plain `Huge` does:
same model, one and a half times bigger.

<Callout type="warning" title="A missing prefixed model has no fallback in the config">

Adding a `Golden` variant to your egg pools means every species in those pools needs a
`Golden<Species>` model in `Storage/Pets`.

Build them before you add the pool entries, or players hatch a pet with nothing to show.

</Callout>

## Mystery power

```lua
mysteryPower = true,
```

A mystery pet's power reads `???` and **copies the player's strongest equipped non-mystery
pet**.

| Equipped | Result |
|---|---|
| A Huge and a Dragon at 50 | The Huge counts as 50 too |
| A Huge and a Cat at 1 | The Huge counts as 1 |
| Only Huges | Every one of them counts as 0 |

That last row is the design: a Huge is worth exactly as much as the best normal pet you
pair it with, so it doubles your best pet rather than being a number of its own. A squad of
nothing but Huges is worth nothing.

`mysteryPower` can sit on a variant, as the three Huge variants do, or on a single species
in `Config/Pets.luau`.

When it is on, `boostMultiplier` is ignored entirely.

## Gradients

`Config/Gradients.luau` is a library of named colour blends used by rarity text and health
bars. Each is a rotation and a list of stops:

```lua
Gold = { rotation = 90, stops = { { 0, { 255, 232, 140 } }, { 1, { 226, 168, 20 } } } },
```

Positions run 0 to 1 and colours are plain RGB triples. Two stops give a simple fade; add
more for a multi-colour blend.

Around two dozen ship, grouped as neutrals, greens, blues, purples, warms and metals. Add
your own and point a rarity at it by name.

## Sounds

`Config/Sounds.luau` is three interface sounds:

```lua
Hover = 10066936758,
Select = 10128760939,
Purchase = 10066947742,
```

Plain asset numbers, not `rbxassetid://` strings. Set one to `0` to turn it off.
