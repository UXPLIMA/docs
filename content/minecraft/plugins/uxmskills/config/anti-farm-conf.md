---
title: anti-farm.conf
order: 502
description: The anti-farm block and its terms.
icon: shield-check
---

```hocon
anti-farm {
  enabled = true
  ceiling = 2.0
  ledger         { enabled = true, factor = 0.0, marks-per-chunk = 4096 }
  replace-window { enabled = true, factor = 0.0, lifetime-ms = 300000 }
  bone-meal      { enabled = true, factor = 0.2, lifetime-ms = 600000 }
  generated      { enabled = true, factor = 0.05 }
  heat-map       { enabled = true, cube-size = 16, free-repeats = 32, decay-per-repeat = 0.97, floor = 0.01,
                   lifetime-ms = 600000, cubes-per-player = 64 }
  pressure       { enabled = true, decay-per-event = 0.9995, recovery-ms = 1200000, floor = 0.05 }
  stillness      { enabled = true, after-ms = 180000, factor = 0.25 }
  adjacency      { enabled = true, minimum-neighbours = 3, factor = 1.1 }
  field-cycle    { enabled = true, cycle-ms = 120000, grown-factor = 1.0, rushed-factor = 0.35 }
  spawner        { enabled = true, factor = 0.1 }
}
```

Every term has its own `enabled`. What each does is in [Anti-farm](../features/anti-farm.md).

Related: [Anti-farm](../features/anti-farm.md)
