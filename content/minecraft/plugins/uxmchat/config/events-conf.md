---
title: events.conf
order: 511
description: The line of each server event, per group and per cause, and the network block.
icon: door-open
---

`modules/events/events.conf`, read at start and on every reload.

```hocon
join {
  line = "@events.join"
  groups { vip = "@events.join-vip" }
}
death {
  line = "@events.death"
  causes { player = "@events.death-player", fall = "@events.death-fall" }
}
unknown-command {
  rewrite = true
  line = "@events.unknown-command"
}
network {
  enabled = false
  join   { line = "@events.network-join" }
  switch { line = "@events.network-switch" }
  quit   { line = "@events.network-quit" }
  resend-after-switch = "missed"
  resend-lines = 10
}
```

Events: `join`, `first-join`, `quit`, `death`, `kick`, `advancement`, `unknown-command`. The placeholders and the
network keys are in [Server lines](../features/events.md).

Related: [Server lines](../features/events.md)
