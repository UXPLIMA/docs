---
title: The toolchain
order: 7
description: Rojo, Aftman, StyLua and Selene, for buyers who want to work on disk.
icon: wrench
---

You do not need any of this to use a system. Drag the folder into Studio, edit the
settings there, done.

This page is for developers who would rather work in an editor with the files on disk,
which is how the systems themselves are built.

## Aftman

Tool versions are pinned in `aftman.toml` at the repository root. One command installs
exactly the versions the project expects:

```
aftman install
```

Pinning matters because a formatter that changes its defaults between versions turns
every file into a diff.

## Rojo

Rojo syncs files on disk into an open Studio place, in both directions.

```
rojo serve
```

Then in Studio, open the Rojo plugin and connect to `localhost:34872`. Edits to `.luau`
files appear in Studio when you save.

| Command | What it does |
|---|---|
| `rojo serve` | Live two-way sync with Studio |
| `rojo serve --port 34873` | The same, when the default port is held by a stale process |
| `rojo build -o out.rbxlx` | Builds the place file, which also checks the tree and the Luau syntax |

`rojo build` is the quickest way to find out whether a change parses, without opening
Studio.

## StyLua

The formatter. Its configuration is in `stylua.toml`: tabs, 120 columns, double quotes.

```
stylua src              format in place
stylua --check src      verify without writing, for CI
```

## Selene

The linter, configured for the Roblox standard library in `selene.toml`.

```
selene src
```

A clean run reports `0 errors, 0 warnings`. Mark an argument you deliberately do not use
with a leading underscore, `_player`, and Selene stops asking about it.

## The project file

`default.project.json` maps disk folders onto Roblox services. The package folder maps to
`ServerScriptService`, which is the same place you would drag it by hand. Some systems
also ship `test.project.json` or `ship.project.json` for a test place and a
customer-facing build.

## Working without any of it

Everything above is optional. The package in Studio is the complete system: you can edit
`Settings.luau` in the Studio script editor, press Play, and never install a tool.

The reason to go to disk is version control. Studio's own history is not a substitute for
a repository when you are about to change a reward ladder that real players depend on.
