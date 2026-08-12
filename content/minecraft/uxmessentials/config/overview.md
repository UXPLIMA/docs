---
title: Overview
order: 1140
description: 'uxmEssentials keeps its settings in HOCON files (.conf), not YAML. If you
  have configured other plugins with config.yml, the ideas are the same (keys, values,
  lists) but the syntax is a little friendlier and the layout is split by module rather
  than piled into one giant file.'
---

This page explains how the config system is organised, where each file lives, and how
to edit and reload it safely.

---

## Why HOCON, not YAML

Everything player-facing and every tunable resolves through typed configuration
loaded with Configurate. That gives us a few practical wins over YAML:

- **Indentation doesn't matter.** Structure comes from `{ }` braces, not from
  counting spaces. A stray space can't silently reparent a key.
- **Comments survive.** Lines starting with `#` are part of the file, so the shipped
  defaults document themselves right next to each key.
- **Values are typed.** A number is a number and a duration is a duration: the plugin
  validates them on load and tells you exactly which key is wrong instead of failing
  cryptically.

<Callout type="warning" title="HOCON syntax rules">

HOCON is forgiving about **whitespace and indentation** but strict about
**structure**:

- Blocks open and close with **braces**: `storage { … }`. Every `{` needs a `}`.
- **Comments start with `#`** and run to the end of the line.
- String values are usually quoted: `backend = "sqlite"`. Numbers and `true`/`false`
  are not: `port = 3306`, `enabled = true`.
- Lists use square brackets: `disabled-worlds = ["arena", "spawn"]`.
- Keys are **kebab-case** (`read-pool-size`, `default-locale`): never camelCase.

If a file won't load after an edit, you almost always have a missing `}` or a stray
quote. The server log names the file and line.

</Callout>

---

## Where everything lives

On first run uxmEssentials extracts its default configuration into
`plugins/uxmEssentials/`. The tree looks like this:

```text
plugins/uxmEssentials/
├── config.conf                     # globals only — storage, locale, network, claims, links…
├── text-input.conf                 # how every "type something" prompt captures your answer
├── modules/
│   ├── teleport/
│   │   ├── config.conf             # enabled + teleport tunables
│   │   ├── rtp.conf                # /rtp safe-search (a lifted-out big block)
│   │   └── gui/
│   │       └── teleport-settings.conf
│   ├── economy/
│   │   ├── config.conf
│   │   ├── currencies.conf         # per-currency definitions
│   │   └── gui/ …
│   ├── communication/
│   │   ├── config.conf
│   │   ├── join-quit.conf
│   │   ├── announcer.conf
│   │   ├── info-pages.conf
│   │   ├── advancements.conf
│   │   └── gui/ …
│   └── … one folder per module (homes, warps, kits, moderation, …)
├── messages/
│   ├── messages_en.conf            # player-facing text (English)
│   └── messages_tr.conf            # player-facing text (Turkish)
├── commands/
│   └── commands.conf               # rename / realias / disable any command
├── menus/
│   └── example.conf                # your own custom GUIs
└── data/
    └── uxmessentials.db            # the SQLite database (default backend)
```

`text-input.conf` is the one root file besides `config.conf`. Anywhere the plugin asks
you to type something (a home name, a warp password, a loan amount) it decides **how**
the prompt appears: `anvil`, `chat`, `sign` or `dialog`. Set a global `default-mode` and
override any single prompt under `modes`. Every prompt works in every mode, so changing
one is always safe, and Bedrock players get a native form regardless. See
[the menu engine page](../menus/engine.md#asking-the-player-to-type-something).

The shape is the whole design: **global settings live in one small file, and each
feature module owns its own folder.** You never wade through unrelated settings to
tune one feature.

<Callout type="danger" title="There is no `modules.conf` file">

A common misconception (carried over from older notes) is that a central
`modules.conf` switchboard turns modules on and off. **It does not exist.** It is
not shipped and never created. Each module is enabled or disabled by the
**`enabled` key at the top of its own `modules/<module>/config.conf`**. See
[Per-Module Config](per-module.md).

</Callout>

### The sub-files inside a module folder

Most modules need only their `config.conf`. A few large features lift a big block out
into a sibling file so the main file stays readable:

- `modules/teleport/rtp.conf`: the `/rtp` safe-search tuning.
- `modules/economy/currencies.conf`: every currency definition.
- `modules/communication/{join-quit,announcer,info-pages,advancements}.conf`: the four
  communication feature blocks.

The `gui/` sub-folder holds one file per management GUI panel: the in-game editors you
reach through `/uxmess gui`. You rarely edit these by hand; they are styled the same way
as [custom menus](../menus/engine.md).

---

## First-run extraction and edits

- Files are written **once**, from the bundled defaults, and **never overwritten**.
  Your edits, comments and formatting survive restarts and plugin upgrades.
- **Delete a single key** and it falls back to its built-in default on next load.
- **Delete a whole file** and it is regenerated from defaults.
- `commands/commands.conf` is generated from the live command list on first run and then
  left untouched.

### What an update does to your files

When a new version adds a setting, that setting is **appended to your file** as a
commented block at the end, under a header naming the version it came from:

```hocon
# -----------------------------------------------------------------------------
# Added by the update to 0.5.1. These settings are new in this version, so your
# file did not have them yet. The values below are the shipped defaults, and
# nothing above this line was touched. ...
# -----------------------------------------------------------------------------
autotool {
    enabled=true
}
```

Nothing above that header is rewritten, and the appended values are the shipped
defaults, so **your server behaves exactly the same after the update as before it**. You
can edit the new keys where they are, or move them up into the matching block: HOCON
merges repeated blocks, so both read the same.

A key you **deleted on purpose is not put back**. The plugin keeps a copy of the
defaults it last shipped you in a hidden `.defaults/` folder inside the plugin folder,
and compares against that, which is how it tells "new in this version" from "the admin
removed this". Leave that folder alone; it is bookkeeping, not something to edit.

<Callout type="note" title="The first update after 0.5.1">

`.defaults/` is created the first time you run 0.5.1 or newer. That run records
what you have and merges nothing, so the appending starts from the update after it.

</Callout>

---

## How to edit and reload

1. Open the relevant `.conf` file in a plain-text editor (VS Code, Notepad++, `nano`).
2. Make your change, keeping braces and quotes balanced.
3. Save the file.
4. Apply it in-game:

```text
/uxmess reload <module>     # reload just one module (fast, targeted)
/uxmess reload              # reload every module
```

Reload re-reads your config tree and message catalogs from disk, off the main thread,
then runs each module's own re-read step and reports one line per step: `[OK]` for a
step whose new values are live now, `[RESTART]` for a module that builds its listeners
and commands once at startup, and `[FAIL]` for a file it could not read. Configuration
is swapped atomically, so players in-flight are never left half-configured.
**Reloading never deletes stored data**: disabling a module leaves its database rows
intact.

<Callout type="tip" title="Some edits need a restart, and reload says so">

Turning a module or a mechanic on or off changes **what is wired**, and wiring
happens once when the server starts. Those edits show up as `[RESTART]` in the
reload output rather than quietly doing nothing. Message and text edits, and
anything reported `[OK]`, apply immediately.

</Callout>

---

## Which page covers what

| Config concern | File | Page |
|---|---|---|
| Storage / database, default locale, cross-server, server links, update check, map markers | `config.conf` | [config.conf (Globals)](global-config.md) |
| Enabling/disabling a module and its tunables | `modules/<m>/config.conf` (+ sub-files) | [Per-Module Config](per-module.md) |
| Renaming, re-aliasing or disabling a command | `commands/commands.conf` | [Renaming Commands](commands-conf.md) |
| Player-facing text and languages | `messages/messages_<lang>.conf` | [Messages & Languages](messages.md) |
| Colours, glyphs and tone of chat/GUI text | catalogs + `menus/*.conf` | [UI Style](ui-style.md) |
| Custom GUIs | `menus/*.conf` | [Custom Menu Engine](../menus/engine.md) |

---

## Next Steps

- [config.conf (Globals)](global-config.md): storage, locale, and network settings.
- [Per-Module Config](per-module.md): enable/disable and tune each feature.
- [Modules & Reloading](../modules/index.md): the full module roster and reload model.
- [Permission Reference](../permissions/reference.md): the nodes that gate every feature.
