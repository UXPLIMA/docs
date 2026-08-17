---
title: Flags
order: 230
description: Toggling the claim-wide rules — PvP, explosions, fire, mob spawning.
icon: flag
---

## How to Open

1. Open claim management
2. Click **Claim Flags** (`changeFlag`)

---

## What Is This Menu For?

Flags are global rules that control the "physics" of your claim:

- Whether explosions work
- Whether fire spreads
- Whether mobs spawn
- Whether PvP is allowed

---

## How Flags Work

Each flag appears as an item in the menu:

- **Green** = Flag is ON (behavior allowed)
- **Red** = Flag is OFF (behavior blocked)

**Click to toggle** between enabled and disabled.

---

## All 31 Flags

### 💥 Security & Damage

| Flag               | What It Controls         |
|--------------------|--------------------------|
| `PVP`              | Player vs player combat  |
| `CREEPER_DAMAGE`   | Creeper explosion damage |
| `WITHER_DAMAGE`    | Wither boss damage       |
| `FIRE_SPREAD`      | Fire spreading to blocks |
| `LIGHTNING_DAMAGE` | Lightning fire/damage    |
| `MOB_GRIEFING`     | Mobs changing blocks     |

### 🌍 World Mechanics

| Flag                | What It Controls             |
|---------------------|------------------------------|
| `FLUID_FLOW`        | Water and lava spreading     |
| `PISTON_PUSH`       | Pistons moving blocks        |
| `LEAF_DECAY`        | Leaves disappearing          |
| `ICE_MELT`          | Ice melting                  |
| `BLOCK_FADE`        | Coral dying, farmland drying |
| `BLOCK_FORM`        | Snow/ice forming naturally   |
| `ENTITY_BLOCK_FORM` | Entities creating blocks     |
| `STRUCTURE_GROW`    | Trees/mushrooms growing      |
| `NATURE_SPREAD`     | Grass/vines spreading        |

### 🐾 Mob Spawning

| Flag             | What It Controls                 |
|------------------|----------------------------------|
| `MOB_SPAWNING`   | All mob spawning (master switch) |
| `SPAWN_ANIMALS`  | Passive mob spawning             |
| `SPAWN_MONSTERS` | Hostile mob spawning             |
| `SPAWN_PHANTOMS` | Phantom spawning                 |

### 🦴 Mob Behavior

| Flag               | What It Controls         |
|--------------------|--------------------------|
| `ENDERMAN_PICKUP`  | Endermen picking blocks  |
| `ZOMBIE_DOOR_BREAK`| Zombies breaking doors   |
| `SILVERFISH_INFEST`| Silverfish infesting     |

### 🧱 Block Mechanics

| Flag          | What It Controls        |
|---------------|-------------------------|
| `CORAL_FADE`  | Coral dying out of water|
| `SCULK_SPREAD`| Sculk spreading         |
| `REDSTONE`    | Redstone mechanisms     |

### 💣 Explosions

| Flag                      | What It Controls            |
|---------------------------|-----------------------------|
| `TNT_EXPLOSIONS`          | TNT block damage            |
| `END_CRYSTAL_DAMAGE`      | End crystal explosions      |
| `BED_EXPLOSION`           | Bed explosions (Nether/End) |
| `FIREBALL_DAMAGE`         | Ghast/Blaze fireballs       |
| `DRAGON_DAMAGE`           | Ender Dragon destruction    |
| `RESPAWN_ANCHOR_EXPLOSION`| Respawn anchor explosions   |

---

## Recommended Settings

### Safe Base

Turn OFF:

- `PVP`
- `TNT_EXPLOSIONS`
- `CREEPER_DAMAGE`
- `FIRE_SPREAD`
- `SPAWN_MONSTERS`
- `ENDERMAN_PICKUP`
- `BED_EXPLOSION`

### PvP Arena

Turn ON:

- `PVP`

Turn OFF everything else to prevent griefing.

### Natural Farm

Turn ON:

- `SPAWN_ANIMALS`
- `STRUCTURE_GROW`
- `NATURE_SPREAD`
- `REDSTONE`

---

## Remember

Flags affect **everyone** - including you, the owner. If you disable `SPAWN_MONSTERS`, you can't spawn mobs either!

---

## Next Steps

- [🛡️ Protection Overview](../protection/overview.md) - How protection works
- [🚩 All Flags Reference](../protection/flags.md) - Detailed flag descriptions
- [🔐 All Permissions](../protection/permissions.md) - Role-based access
