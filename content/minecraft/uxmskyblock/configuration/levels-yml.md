---
title: levels.yml
order: 780
---

`levels.yml` turns an island's total **points** into a **level**. See
[Points & Levels](../progression/points-levels.md) for the gameplay side.

---

## Format

```yaml
# Total points required to reach a level.
requirements:
  1: 100
  2: 250
  3: 500
  4: 1000
  5: 2000
  6: 3500
  7: 5500
  8: 8000
  9: 12000
  10: 18000

# Levels beyond the highest one above are generated automatically.
auto:
  enabled: true
  step: 8000
  multiplier: 1.15
```

---

## `requirements`

Each entry is `level: total points`. An island reaches a level once its **total**
points meet that threshold. Values are cumulative totals, not per-level deltas.

---

## `auto` — endless levels

Hand-writing hundreds of levels is tedious, so anything past the last defined
threshold is generated:

| Key | Meaning |
|-----|---------|
| `enabled` | Turn auto-generation on/off |
| `step` | Base points added for each new level past the last defined one |
| `multiplier` | `step` is multiplied by this each level (1.0 = constant, >1 = curve) |

With `step: 8000` and `multiplier: 1.15`, each level past 10 costs ~15% more than
the previous gap — a smooth difficulty curve that never ends.

<Callout type="tip">

If you want a fixed cap, set `auto.enabled: false`; the highest level in
`requirements` becomes the maximum. Reload with `/is admin reload`.

</Callout>
