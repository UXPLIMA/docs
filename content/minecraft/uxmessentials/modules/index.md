---
title: Modules
order: 94
description: Every feature ships as a module you can switch off. One page each.
---

uxmEssentials is 34 modules behind one jar. Each one owns its commands, its permission nodes, its placeholders
and its config file at `plugins/uxmEssentials/modules/<module>/config.conf`, and each can be switched off on its
own: a disabled module registers no commands, no listeners and no database migrations, and holds no runtime
state.

Set `enabled = false` in a module's `config.conf` and restart, or edit and run `/uxmess reload <module>`.
