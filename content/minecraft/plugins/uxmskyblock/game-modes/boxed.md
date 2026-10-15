---
title: Boxed
order: 70
description: An island played on real land inside a box that every advancement pushes out, with
  growth to buy and a side to grow toward.
icon: package
---

A Boxed island is played on land the server makes the usual way, inside a box. The box starts
small and every advancement one of the island's players makes pushes it out. Each advancement
counts once for an island, whoever on it makes it. Players see where their box ends as a world
border, and nobody builds, breaks or walks outside it.

## Turning it on

1. Make a world the server's usual way, with generated terrain, not with this plugin's empty
   generator. The shipped preset names it `boxed`.
2. Set `enabled = true` in `modules/boxed.conf`.
3. Add `boxed` to `presets.modes` in `modules/presets.conf`.

The shipped preset `boxed` starts with `"uxm:boxed"`. Nothing is built: the island is the land.
The Nether and the End stay closed to a Boxed island until a dimension is named in its preset.

## The land

| Setting | Shipped | What it does |
| --- | --- | --- |
| `land.from` | `template` | `template` lays the same land on every new island; `world` leaves each on the land its world made |
| `land.template.x`, `.z` | `1000000` | Where the template land is, in the island's own world |
| `land.ahead` | `16` | How far past the box the land is copied, from 0 to 128 |

With a template, no island starts on better land than another. An island keeps the land it was
made on, and one made on the template keeps getting it as its box grows, even after `from`
changes. Pick a template no island reaches, and leave it where it is once islands are made on it.
Keep `ahead` above what one advancement grows the box by, so the land is laid before the box
reaches it.

## The box

| Setting | Shipped | What it does |
| --- | --- | --- |
| `box.start-radius` | `5` | How far the box reaches when the island is made. 5 is a box 11 across |
| `box.blocks-per-advancement` | `1` | How far an advancement pushes the box out, unless it has a worth of its own |
| `box.max-radius` | `400` | How far the box can ever reach. Keep it inside half the grid spacing |
| `border-every` | `1s` | How often each player is shown where their box ends |
| `bypass-permission` | `""` | A node whose holder is not kept inside a box. Empty for nobody |
| `visitors-earn` | `false` | Whether an advancement made on another island grows the player's own box |

## What an advancement is worth

`advancements` gives an advancement a worth of its own by its full key, as
`"minecraft:story/mine_diamond" = 5`. `0` grows nothing. The rest are scored by `scoring`:

| Key | Shipped | What it does |
| --- | --- | --- |
| `by` | `depth` | `depth` grows the box for each step the advancement lies from its tab's root; `flat` grows it the same for each |
| `root` | `0` | What a tab's first advancement is worth, which every player gets on joining |
| `recipes` | `0` | What a recipe unlocked is worth |
| `hidden` | `0` | What an advancement with no place in the advancements screen is worth |

Advancements a data pack adds count as the game's own do, whatever namespace they live in.
`ignore` lists key prefixes worth nothing, `["minecraft:recipes/"]` as shipped. A namespace alone,
as `"mypack:"`, leaves out every advancement that pack adds.

## Buying growth

`/is boxbuy` grows the box on top of what advancements earn. Each purchase grows it `buy.blocks`
and asks every line of `buy.lines`, had or taken, from the buyer or from the island bank, which
only a member allowed to spend from it pays out of:

```hocon
buy {
    blocks = 1
    lines = [ "take money 2500" ]
    most = 50
}
```

`most` is how many times an island may buy, `0` for no end. `lines = []` turns buying off.

A box with a side chosen with `/is boxside` grows toward it, bought or earned: the opposite edge
stays where it is.

## Commands

| Command | What it does |
| --- | --- |
| `/is boxbuy` | Buys growth for your Boxed island's box |
| `/is boxside [side]` | Shows or chooses the side your box grows toward |
| `/isadmin boxed setsize <target> <size>` | Sets how far across an island's box is |
| `/isadmin boxed grow <target> <blocks>` | Pushes an island's box out |

The island's edge moves with the box, as it does when an advancement grows it. Each admin verb
asks `uxmskyblock.admin.boxed.<verb>`.
