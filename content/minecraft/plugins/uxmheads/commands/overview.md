---
title: Overview
order: 301
description: The root, renaming it, the console, and who sees what.
icon: list-tree
---

uxmHeads has one command: `/uxmheads`, aliases `/heads` and `/uxmh`. Players use only its `lang` branch; everything
else is for operators. See [Operator commands](admin.md).

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md); a change needs a restart.

Every subcommand runs from the console except a player's own `lang`, `lang set` and `lang reset`. A sender without an
operator node does not see that subcommand; a player who lost `uxmheads.language` still sees the `lang` branch and is
told `command.no-permission`.

The bare `/uxmheads` says to add `help`, and needs `uxmheads.admin.version`.

Related: [Operator commands](admin.md)
