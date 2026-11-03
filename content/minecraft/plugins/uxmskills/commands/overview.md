---
title: Overview
order: 201
description: The three commands, their aliases, the skill shortcuts and how a missing permission is answered.
icon: terminal
---

| Command | Aliases | For |
|---|---|---|
| [`/skills`](skills.md) | `/skill`, `/sk` | Players |
| [`/uxmskills`](admin.md) | `/skillsadmin` | Operators, and a player's own language |
| [`/skillsedit`](editor.md) | `/seedit` | The in-game editor |

## Skill shortcuts

`shortcuts` in [commands.conf](../config/commands-conf.md) gives every skill the server has a command of its own:
`/mining`, `/excavation`, `/woodcutting`, `/farming`, `/fishing`, `/combat`, `/archery`, `/defence`, `/agility`,
`/alchemy`, `/enchanting`, and one for each skill file you add. Each opens that skill's window, as `/skills skill`
does. The family is built from the files under `skills/` when the server builds its commands.

```hocon
shortcuts {
  enabled = true
  subcommands {
    mining { name = "kaz" }          # /kaz opens mining instead of /mining
    farming { enabled = false }      # farming has no shortcut
  }
}
```

Every name and alias is in commands.conf; a change needs a restart.

A player node is held by everybody, so its command stays visible and a player without it is refused with
`command.no-permission`. An operator node hides its command.

Related: [Permissions](../permissions/reference.md)
