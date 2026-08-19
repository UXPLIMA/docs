---
title: Particles
order: 215
description: A particle plus exactly the data it requires, checked by the compiler.
icon: sparkles
---

Bukkit's `spawnParticle` takes an untyped `Object` for the particle's extra data, so passing a colour
to a flame or nothing to a dust compiles fine and fails at runtime. `ParticleOptions` is a sealed set
of records that makes the compiler reject that.

```java
Particles.spawn(world, location, ParticleOptions.of(Particle.FLAME), 20);

Particles.spawn(world, location, ParticleOptions.dust(Color.RED, 1.5f), 30);

Particles.spawn(world, location,
        ParticleOptions.dustTransition(Color.RED, Color.BLUE, 1f), 30);

Particles.spawn(world, location,
        ParticleOptions.block(Particle.BLOCK, Material.STONE.createBlockData()), 15);

Particles.spawn(world, location, ParticleOptions.item(sword), 10);
```

| Factory | Particle kind |
|---|---|
| `of(Particle)` | Anything that carries no extra data: flame, heart, smoke |
| `dust(Color, size)` | Coloured dust; size 1.0 is the default scale |
| `dustTransition(from, to, size)` | Dust fading between two colours |
| `block(Particle, BlockData)` | Block-textured: `BLOCK`, `BLOCK_MARKER`, `FALLING_DUST` |
| `item(ItemStack)` | Item-textured |

Each constructor validates that the particle actually takes that payload, so `ParticleOptions.of` on
a particle that needs data fails immediately rather than silently rendering nothing.

## Spread and speed

The four-argument form spawns them all at one point. The full form adds a Gaussian spread and a
speed, the same parameters vanilla uses:

```java
Particles.spawn(world, location, options, 40, 0.5d, 0.5d, 0.5d, 0.02d);
```

## Per-player particles

Pass a `Player` instead of a `World` and only that player sees them.

```java
Particles.spawn(player, location, ParticleOptions.of(Particle.HAPPY_VILLAGER), 5);
```

This is how you show a build outline, a claim boundary or a selection to one person without
decorating the server for everybody standing nearby.

<Callout type="note" title="Particles are a region-threaded concern">

Spawning at a location touches that location's region. On Folia, schedule through
`scheduler.region(location, ...)` rather than calling from wherever you happen to be.

</Callout>
