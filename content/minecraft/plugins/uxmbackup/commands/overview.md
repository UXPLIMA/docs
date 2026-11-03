---
title: Overview
order: 301
description: The root, renaming it, the console, and who sees what.
icon: list-tree
---

uxmBackup has one command: `/uxmbackup`, aliases `/backup` and `/uxmb`. Everything except the language branch is for
operators. See [Operator commands](admin.md).

Every name, alias and subcommand is set in [`commands.conf`](../config/commands-conf.md); a change needs a restart.
A subcommand turned off there answers `command.turned-off` if it is still reached.

`restore` and `hash` run from the console only, whatever permission a player holds. `menu` needs a player. Every other
subcommand runs from both.

A sender without an operator node does not see that subcommand; a player who lost `uxmbackup.language` still sees the
`lang` branch and is told `command.no-permission`.

Related: [Operator commands](admin.md)
