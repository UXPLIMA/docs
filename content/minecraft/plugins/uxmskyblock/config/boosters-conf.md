---
title: boosters.conf
order: 615
description: Timed boosts to spawners, crops, the generator, drops and experience.
icon: zap
---

The file is `plugins/uxmSkyblock/modules/boosters.conf`. Every default below is the shipped value.

## `boosters`

Boosters an island buys or is given: for a while, spawners, crops, the ore generator, mob experience or island worth count for more.

| Key | Default | What it does |
|---|---|---|
| `pause-when-empty` | `true` | true stops a booster's clock while none of the island's members is online. |
| `clean-interval` | `60s` | How often boosters that ran out are taken off. |
| `multiplier-cache-ttl` | `5s` | How long a booster multiplier stays good enough to use without asking the database again. A mob death has to know the multiplier before the event returns, so it reads this cache instead of the database and refreshes it on a background thread. Lower means a booster takes effect sooner after it is bought; higher means fewer queries under a mob farm. |

## `boosters.categories`

One entry per kind of booster.

```text
enabled             false turns that kind off
stack-mode          what a second booster of the same kind does while one runs:
                      DURATION    adds its time; the multiplier stays, up to max-duration
                      MULTIPLIER  adds its multiplier, up to max-multiplier
                      REPLACE     a stronger one takes the place of a weaker one
duration-policy     with MULTIPLIER: INDEPENDENT lets each booster run out on its own,
                    REFRESH starts the whole stack's time again
calculation         ADDITIVE adds what each booster adds (1.5x and 1.5x make 2.0x),
                    COMPOUND multiplies them (1.5x and 1.5x make 2.25x)
max-multiplier      the most a stack of this kind reaches
max-duration        the longest a stack of this kind runs
```

## `boosters.categories.spawner_rate`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` |  |
| `stack-mode` | `DURATION` |  |
| `duration-policy` | `INDEPENDENT` |  |
| `calculation` | `ADDITIVE` |  |
| `max-multiplier` | `5.0` |  |
| `max-duration` | `7d` |  |
| `boosters.categories.crop_growth.enabled` | `true` |  |
| `boosters.categories.crop_growth.stack-mode` | `DURATION` |  |
| `boosters.categories.crop_growth.duration-policy` | `INDEPENDENT` |  |
| `boosters.categories.crop_growth.calculation` | `ADDITIVE` |  |
| `boosters.categories.crop_growth.max-multiplier` | `5.0` |  |
| `boosters.categories.crop_growth.max-duration` | `7d` |  |
| `boosters.categories.ore_generator.enabled` | `true` |  |
| `boosters.categories.ore_generator.stack-mode` | `DURATION` |  |
| `boosters.categories.ore_generator.duration-policy` | `INDEPENDENT` |  |
| `boosters.categories.ore_generator.calculation` | `ADDITIVE` |  |
| `boosters.categories.ore_generator.max-multiplier` | `5.0` |  |
| `boosters.categories.ore_generator.max-duration` | `7d` |  |
| `boosters.categories.mob_exp.enabled` | `true` |  |
| `boosters.categories.mob_exp.stack-mode` | `MULTIPLIER` |  |
| `boosters.categories.mob_exp.duration-policy` | `INDEPENDENT` |  |
| `boosters.categories.mob_exp.calculation` | `ADDITIVE` |  |
| `boosters.categories.mob_exp.max-multiplier` | `4.0` |  |
| `boosters.categories.mob_exp.max-duration` | `3d` |  |
| `boosters.categories.island_worth.enabled` | `true` |  |
| `boosters.categories.island_worth.stack-mode` | `DURATION` |  |
| `boosters.categories.island_worth.duration-policy` | `INDEPENDENT` |  |
| `boosters.categories.island_worth.calculation` | `ADDITIVE` |  |
| `boosters.categories.island_worth.max-multiplier` | `3.0` |  |
| `boosters.categories.island_worth.max-duration` | `3d` |  |
| `boosters.categories.mission_rewards.enabled` | `true` |  |
| `boosters.categories.mission_rewards.stack-mode` | `MULTIPLIER` |  |
| `boosters.categories.mission_rewards.duration-policy` | `REFRESH` |  |
| `boosters.categories.mission_rewards.calculation` | `ADDITIVE` |  |
| `boosters.categories.mission_rewards.max-multiplier` | `3.0` |  |
| `boosters.categories.mission_rewards.max-duration` | `3d` |  |
