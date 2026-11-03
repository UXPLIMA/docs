---
title: protection.conf
order: 602
description: The protection block, key by key.
icon: shield
---

```hocon
protection {
  shared-address-hosts   = []
  shared-address-sources = []
  bans {
    steps-minutes       = [5, 30, 60, 1440]
    failures            = 3
    forgive-after-hours = 24
  }
  connections {
    per-address      = 5
    window-seconds   = 60
    exempt-addresses = []
  }
  accounts {
    per-address      = 3
    window-hours     = 24
    exempt-addresses = []
  }
  countries {
    mode            = off
    list            = []
    allow-addresses = []
    unknown         = allow
    account-id      = ""
    licence-key     = ""
    refresh-days    = 7
  }
  captcha {
    when                   = off
    after-failures         = 3
    length                 = 5
    characters             = "ABCDEFGHJKMNPQRSTUVWXYZ23456789"
    failure-memory-minutes = 60
    java                   = dialog
  }
}
```

Every key is explained in [Protection](../features/protection.md) and [Captcha](../features/captcha.md).

Related: [Protection](../features/protection.md)
