---
title: Configuration
order: 114
description: Overview, config.conf (Globals), Per-Module Config, Renaming Commands,
  Messages & Languages and UI Style.
---

- [Overview](overview.md): uxmEssentials keeps its settings in HOCON files (.conf), not YAML. If you have configured other plugins with config.yml, the ideas are the same (keys, values, lists) but the syntax is a little friendlier and the layout is split by module rather than piled into one giant file.
- [config.conf (Globals)](global-config.md)
- [Per-Module Config](per-module.md): Every feature in uxmEssentials is a module with its own folder under plugins/uxmEssentials/modules/. All of a feature's tunables live in that folder, and the module is switched on or off by the first key in its own config.conf. There is no central switchboard file (see the warning below).
- [Renaming Commands](commands-conf.md): plugins/uxmEssentials/commands/commands.conf lets you rename, re-alias, or disable any command without touching code or waiting for an update. It is one global file, keyed by each command's stable id.
- [Messages & Languages](messages.md): Every player-facing line in uxmEssentials (command feedback, GUI titles, cooldown notices, everything) comes from a per-language message catalog under plugins/uxmEssentials/messages/. No user-visible text is hardcoded, so you can rewrite any line and translate the whole plugin.
- [UI Style](ui-style.md)
