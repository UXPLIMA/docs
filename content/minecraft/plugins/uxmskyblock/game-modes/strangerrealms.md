---
title: StrangerRealms
order: 90
description: An island of real land with an Upside Down in place of the Nether, mirrored and
  distressed, with lights, switches and a compass that reach across the veil.
icon: moon
---

A StrangerRealms island is real land, with an Upside Down in place of the Nether. The Upside Down
is the island's land mirrored block for block and distressed: the grass dead, the trees bare, the
flowers withered. It is laid when the island is made, in the world `modules/dimensions.conf` names
for the Nether, so the Nether of a StrangerRealms island is its Upside Down.

## Turning it on

1. Make a world the server's usual way, with generated terrain. The shipped preset names it
   `realms`.
2. Set `enabled = true` in `modules/strangerrealms.conf`.
3. Add `stranger_realms` to `presets.modes` in `modules/presets.conf`.

The shipped preset `stranger_realms` starts with `"uxm:upside-down"`. Nothing is built: the island
is the land, and its Upside Down is mirrored from it.

## The Upside Down

| Setting | Shipped | What it does |
| --- | --- | --- |
| `upside-down.radius` | `48` | How far from the centre the land is mirrored. Every chunk it reaches is copied whole |
| `upside-down.palette` | | How the mirror distresses what it copies |
| `upside-down.late-copy` | plain ground | What a mirror laid after the island was made copies |

A palette rule is `FROM:TO`: a block, or a name with one `*` such as `*_LEAVES`, turned into
another block. The first rule that matches wins, and a block no rule matches is mirrored as it is.

```hocon
palette = [ "GRASS_BLOCK:MYCELIUM", "*_LEAVES:AIR", "*_LOG:STRIPPED_DARK_OAK_LOG", "POPPY:WITHER_ROSE" ]
```

The island grows past the radius with its members and its size, and its land there is mirrored
the first time it loads. By then players may have built on it, and a block copied across would be
had twice. So a late mirror copies only the ground `late-copy` lists and lays air for the rest:
what players put down, ore, and anything that holds things. Keep that list to plain ground.

## Creatures

The Upside Down is the Nether's world, so what would be born there is Nether-born. `mobs` turns it
into the creature of the overworld it has become.

- `reasons`: the ways of being born that are turned, as `NATURAL`, `CHUNK_GEN` and `SPAWNER`. A
  creature a command or a plugin brings is left alone.
- `turn`: rules `FROM:TO`, as `"PIGLIN:ZOMBIE"`. `TO` may be `NONE`, which keeps the creature from
  being born.
- `effects`: what every creature born there carries.

## Across the veil

- **The glimmer.** A light set on the land shines through at the same place in the Upside Down,
  and one set in the Upside Down shines through on the land, for as long as it stands. It is light
  without a block, so nothing can be taken or built on it. `glimmer.lights` lists the blocks that
  are lights, and `glimmer.level` how bright it is.
- **Switches.** A switch of `switches.blocks` used on one side acts at the same place on the other:
  a lever or a button passes on its power, and a door, trapdoor or gate stands open or shut as the
  one used does.
- **The warped compass.** Held on a StrangerRealms island, it points at the nearest player on the
  other side of the veil. With nobody over there it spins. It is crafted from
  `warped-compass.ingredients`, a compass and a warped fungus as shipped, in any order.

## How far an island reaches

An island starts at what its size upgrade gives it and reaches `claim.per-member` blocks further
for every member past the first, up to `claim.max-radius`. A size bought past the limit is kept.

## The border

The border of the StrangerRealms land holds the furthest StrangerRealms island with
`border.margin` blocks of room around it, and grows as islands are made, on this server or any
other that shares the database. It is set only in `border.worlds`, or in every world only
StrangerRealms presets use when that is empty, so no island of another mode is ever cut off. The
Nether is shared with every mode, so the Upside Down is never given one.

| Setting | Shipped | What it does |
| --- | --- | --- |
| `border.check-every` | `60s` | How often the furthest island is read and the border moved |
| `border.minimum`, `border.maximum` | `2048`, `29999984` | The narrowest and the widest it is |
| `border.per-player` | `0` | How much wider it is for each player on the network |
| `border.transition` | `30s` | How long it takes to move to a new width |

## Commands

| Command | What it does |
| --- | --- |
| `/isadmin realms remirror <target> [chunk_x chunk_z]` | Lays an island's Upside Down again from its land, the whole island or one chunk |

A remirror copies what a late mirror copies, the plain ground of `late-copy`, and air for the
rest, so nothing anybody built is had twice. It asks `uxmskyblock.admin.realms.remirror`.
