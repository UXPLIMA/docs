---
title: ownership.conf
order: 503
description: The ownership block, key by key.
icon: badge-check
---

```hocon
ownership {
  rule = automatic
  when-every-source-fails = cracked
  failed-memory-seconds = 300
  premium-password-fallback = true
  claim-minutes = 10
  domains {
    premium = []
    cracked = []
    cracked-mark = "+"
    mark-at = end
  }
  uuid-scheme = keep-first
  conflict = rename
  rename-mark = "_"
}
```

Every key is explained in [Premium names](../features/premium.md).

Related: [Premium names](../features/premium.md)
