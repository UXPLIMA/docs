---
title: custom-commands.conf
order: 612
description: The keys of a custom command and the three shipped ones.
icon: square-terminal
---

`modules/custom-commands/custom-commands.conf`, read at start; a change needs a restart.

```hocon
commands {
  report {
    description = "Report a player to staff"
    permission = "uxmchat.command.report"
    min-args = 2
    usage = "<bad>Usage: /report <player> <reason>"
    actions = [
      "[player] helpop Report on {arg1}: {args2}"
      "[message] <good>Thank you. Staff will look into it."
    ]
    cooldown-seconds = 60
  }
}
```

The keys and the argument tokens are in [Custom commands](../features/custom-commands.md).

Related: [Custom commands](../features/custom-commands.md)
