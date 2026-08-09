---
title: All Claim Flags
order: 310
---

## Quick Reference Table

| Flag | Default | What It Controls |
|------|---------|------------------|
| PVP | OFF | Player vs player combat |
| KEEP_INVENTORY | OFF | Keep items and XP on death |
| CREEPER_DAMAGE | ON | Creeper explosion block damage |
| WITHER_DAMAGE | ON | Wither boss block damage |
| FIRE_SPREAD | ON | Fire spreading to blocks |
| LIGHTNING_DAMAGE | OFF | Lightning fire and damage |
| MOB_GRIEFING | OFF | Mobs modifying blocks |
| FLUID_FLOW | ON | Water/lava spreading |
| PISTON_PUSH | ON | Pistons moving blocks |
| LEAF_DECAY | ON | Leaves disappearing |
| ICE_MELT | ON | Ice melting near heat |
| BLOCK_FADE | ON | Blocks fading (coral, farmland) |
| BLOCK_FORM | ON | Natural block formation |
| ENTITY_BLOCK_FORM | ON | Entities creating blocks |
| STRUCTURE_GROW | ON | Trees/mushrooms growing |
| NATURE_SPREAD | ON | Grass/vines spreading |
| MOB_SPAWNING | ON | All mob spawning (master switch) |
| SPAWN_ANIMALS | ON | Passive mob spawning |
| SPAWN_MONSTERS | ON | Hostile mob spawning |
| SPAWN_PHANTOMS | ON | Phantom spawning |
| ENDERMAN_PICKUP | ON | Endermen picking up blocks |
| ZOMBIE_DOOR_BREAK | ON | Zombies breaking doors |
| SILVERFISH_INFEST | ON | Silverfish infesting blocks |
| CORAL_FADE | ON | Coral dying outside water |
| SCULK_SPREAD | ON | Sculk spreading |
| REDSTONE | ON | Redstone mechanisms |
| TNT_EXPLOSIONS | ON | TNT block damage |
| END_CRYSTAL_DAMAGE | ON | End crystal explosions |
| BED_EXPLOSION | ON | Bed explosions (Nether/End) |
| FIREBALL_DAMAGE | ON | Ghast/Blaze fireball damage |
| DRAGON_DAMAGE | ON | Ender Dragon damage |
| RESPAWN_ANCHOR_EXPLOSION | ON | Respawn anchor explosions |

---

## 💥 Security & Damage Flags

### PVP

**Controls:** Player vs player combat

| ON | OFF |
|----|-----|
| Players can hurt each other | Players cannot hurt each other |

**Use case:** OFF for peaceful zones, ON for PvP arenas

---

### KEEP_INVENTORY

**Controls:** Whether players keep their inventory and XP when they die

| ON | OFF |
|----|-----|
| Players keep items and XP on death | Normal death drops |

**Use case:** ON for safe zones where you don't want players to lose their items

---

### CREEPER_DAMAGE

**Controls:** Whether creeper explosions destroy blocks

| ON | OFF |
|----|-----|
| Creepers destroy your builds | Creepers explode but cause no block damage |

**Recommendation:** OFF to protect builds

---

### WITHER_DAMAGE

**Controls:** Wither boss and wither skull block damage

| ON | OFF |
|----|-----|
| Wither destroys blocks | Wither cannot destroy blocks |

---

### FIRE_SPREAD

**Controls:** Whether fire spreads to adjacent blocks

| ON | OFF |
|----|-----|
| Fire spreads naturally | Fire stays in place, doesn't spread |

**Recommendation:** OFF for wooden builds

---

### LIGHTNING_DAMAGE

**Controls:** Lightning strikes starting fires

| ON | OFF |
|----|-----|
| Lightning can start fires | Lightning causes no fires |

---

### MOB_GRIEFING

**Controls:** Mobs modifying blocks (general)

| ON | OFF |
|----|-----|
| Mobs can modify blocks | Mobs cannot modify blocks |

**Note:** For specific mob behaviors, see the Mob Behavior section below.

---

## 🌍 World Mechanics Flags

### FLUID_FLOW

**Controls:** Water and lava spreading

| ON | OFF |
|----|-----|
| Fluids flow normally | Fluids stay frozen in place |

---

### PISTON_PUSH

**Controls:** Pistons moving blocks

| ON | OFF |
|----|-----|
| Pistons work normally | Pistons cannot move blocks |

**Keep ON** for redstone builds

---

### LEAF_DECAY

**Controls:** Leaves decaying after tree is cut

| ON | OFF |
|----|-----|
| Leaves disappear naturally | Leaves stay forever |

---

### ICE_MELT

**Controls:** Ice melting near heat/light

| ON | OFF |
|----|-----|
| Ice melts near torches | Ice never melts |

**Turn OFF** for ice builds in warm areas

---

### BLOCK_FADE

**Controls:** Blocks "fading" over time

| ON | OFF |
|----|-----|
| Coral dies, farmland dries | Blocks don't fade |

Affects: Coral, farmland, snow layers

---

### BLOCK_FORM

**Controls:** Natural block formation

| ON | OFF |
|----|-----|
| Snow appears, water freezes | No natural block creation |

---

### ENTITY_BLOCK_FORM

**Controls:** Entities creating blocks

| ON | OFF |
|----|-----|
| Snow golems leave snow trails | Entities can't create blocks |

---

### STRUCTURE_GROW

**Controls:** Trees and giant mushrooms growing

| ON | OFF |
|----|-----|
| Saplings grow into trees | Trees don't grow |

---

### NATURE_SPREAD

**Controls:** Vegetation spreading

| ON | OFF |
|----|-----|
| Grass, mycelium, vines spread | No vegetation spreading |

---

## 🐾 Mob Spawning Flags

### MOB_SPAWNING

**Controls:** ALL mob spawning (master switch)

| ON | OFF |
|----|-----|
| Mobs can spawn | No mobs spawn at all |

<Callout type="warning" title="Master Switch">

When OFF, this overrides all specific spawn flags below. All natural mob spawning will be disabled.

</Callout>

---

### SPAWN_ANIMALS

**Controls:** Passive mob spawning

| ON | OFF |
|----|-----|
| Cows, pigs, chickens spawn naturally | No natural animal spawning |

---

### SPAWN_MONSTERS

**Controls:** Hostile mob spawning

| ON | OFF |
|----|-----|
| Zombies, skeletons spawn | No hostile mob spawning |

**Turn OFF** for safe zones

---

### SPAWN_PHANTOMS

**Controls:** Phantom spawning

| ON | OFF |
|----|-----|
| Phantoms spawn when you don't sleep | No phantom spawning |

**Recommendation:** OFF (phantoms are annoying!)

---

## 🦴 Mob Behavior Flags

### ENDERMAN_PICKUP

**Controls:** Endermen picking up and placing blocks

| ON | OFF |
|----|-----|
| Endermen can steal blocks | Endermen cannot pick up blocks |

**Recommendation:** OFF to protect builds

---

### ZOMBIE_DOOR_BREAK

**Controls:** Zombies breaking wooden doors

| ON | OFF |
|----|-----|
| Zombies can break doors | Zombies cannot break doors |

---

### SILVERFISH_INFEST

**Controls:** Silverfish infesting stone blocks

| ON | OFF |
|----|-----|
| Silverfish can hide in stone | Silverfish cannot infest blocks |

---

## 🧱 Block Mechanics Flags

### CORAL_FADE

**Controls:** Coral blocks dying when not in water

| ON | OFF |
|----|-----|
| Coral dies outside water | Coral stays alive anywhere |

**Turn OFF** for decorative coral builds

---

### SCULK_SPREAD

**Controls:** Sculk spreading from sculk catalysts

| ON | OFF |
|----|-----|
| Sculk spreads when mobs die | Sculk does not spread |

---

### REDSTONE

**Controls:** Redstone mechanisms functioning

| ON | OFF |
|----|-----|
| Redstone works normally | Redstone is disabled |

**Keep ON** for redstone contraptions

---

## 💣 Explosion Flags

### TNT_EXPLOSIONS

**Controls:** Whether TNT explosions destroy blocks

| ON | OFF |
|----|-----|
| TNT destroys blocks | TNT does not destroy blocks |

**Note:** Players still need IGNITE permission to light TNT

---

### END_CRYSTAL_DAMAGE

**Controls:** End crystal explosion damage

| ON | OFF |
|----|-----|
| End crystals destroy blocks | End crystals cause no block damage |

---

### BED_EXPLOSION

**Controls:** Bed explosions in Nether and End

| ON | OFF |
|----|-----|
| Beds explode and destroy blocks | Beds cause no explosion damage |

---

### FIREBALL_DAMAGE

**Controls:** Ghast and Blaze fireball damage

| ON | OFF |
|----|-----|
| Fireballs destroy blocks | Fireballs cause no block damage |

---

### DRAGON_DAMAGE

**Controls:** Ender Dragon destruction

| ON | OFF |
|----|-----|
| Dragon can destroy blocks | Dragon cannot destroy blocks |

---

### RESPAWN_ANCHOR_EXPLOSION

**Controls:** Respawn anchor explosions in Overworld/End

| ON | OFF |
|----|-----|
| Anchors explode and destroy blocks | Anchors cause no explosion damage |

---

## Recommended Configurations

### Safe Base

```yaml
PVP: OFF
KEEP_INVENTORY: ON
TNT_EXPLOSIONS: OFF
CREEPER_DAMAGE: OFF
WITHER_DAMAGE: OFF
FIRE_SPREAD: OFF
MOB_GRIEFING: OFF
ENDERMAN_PICKUP: OFF
ZOMBIE_DOOR_BREAK: OFF
SPAWN_MONSTERS: OFF
SPAWN_PHANTOMS: OFF
BED_EXPLOSION: OFF
END_CRYSTAL_DAMAGE: OFF
RESPAWN_ANCHOR_EXPLOSION: OFF
```

### Natural Farm

```yaml
SPAWN_ANIMALS: ON
STRUCTURE_GROW: ON
NATURE_SPREAD: ON
FLUID_FLOW: ON
REDSTONE: ON
```

### PvP Arena

```yaml
PVP: ON
# Keep explosion flags OFF to prevent griefing
TNT_EXPLOSIONS: OFF
CREEPER_DAMAGE: OFF
```

### Redstone Lab

```yaml
REDSTONE: ON
PISTON_PUSH: ON
FLUID_FLOW: ON
MOB_SPAWNING: OFF
```

---

## Next Steps

- [🔐 All Permissions](../protection/permissions.md) - Complete permission reference
- [🏠 Claim Management](../menus/claim-management.md) - How to manage your claim
