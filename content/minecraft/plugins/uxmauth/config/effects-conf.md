---
title: effects.conf
order: 505
description: The effects block, its moments, and per-server lines.
icon: sparkles
---

```hocon
effects {
  join-unregistered = [ "[subtitle] @join.register-title | @join.register-subtitle" ]
  join-registered   = [ "[subtitle] @join.login-title | @join.login-subtitle" ]
  join-code         = [ "[subtitle] @join.code-title | @join.code-subtitle" ]
  reminder-register = [ "[actionbar] @reminder.register" ]
  reminder-login    = [ "[actionbar] @reminder.login" ]
  reminder-code     = [ "[actionbar] @reminder.code" ]
  wrong-password    = [ "[sound] block.note_block.bass 0.8 0.6" ]
  registered        = [ "[title] @welcome.registered", "[sound] block.note_block.chime 0.6 1.2" ]
  logged-in         = [ "[title] @welcome.logged-in", "[sound] block.note_block.chime 0.6 1.2" ]
  logged-in-password = []
  logged-in-premium  = []
  logged-in-bedrock  = []
  logged-in-session  = []
  logged-in-forced   = []
  conflict = []
  timeout = []
  too-many-attempts = []
  banned = []
  logged-out        = [ "[sound] block.note_block.hat 0.8 1.0" ]
  password-changed  = [ "[message] @change.done", "[sound] block.note_block.chime 0.6 1.2" ]
  servers { }
}
```

Every moment is explained in [Effects](../features/effects.md).

Related: [Effects](../features/effects.md)
