---
title: Core Concepts
order: 920
description: A quick tour of the five ideas every uxmEssentials admin should know.
  Once these click, everything else in the wiki is just detail.
---

---

## 1. Modules

uxmEssentials is **34 independent feature modules** — teleport, homes, warps,
economy, kits, moderation, holograms, worlds and so on. Each one is self-contained.

The important part: **a disabled module wires nothing.** No commands are
registered, no listeners run, no database tables are created, no background tasks
tick, and it holds zero runtime state. Turning a module off genuinely costs you
nothing.

Every module is toggled by the `enabled` key at the top of its own config file:

```
/uxmess reload <module>     # re-read files, scoped to one module
/uxmess reload              # re-read files for everything
/uxmess status              # see which modules are on
```

`/uxmess reload` re-reads your config tree and message catalogs from disk (off the
main thread) and then runs each module's own re-read step. It reports what actually
happened, one line per step:

| Line | Meaning |
|------|---------|
| `[OK]` | Re-read, and the new values are live now |
| `[RESTART]` | That module builds its listeners and commands once at startup, so your edit waits for a server restart |
| `[FAIL]` | The file could not be read (usually a HOCON syntax error); the previous values are still in force |

Most modules wire their listeners and commands once when the server starts, so
changes to **what is wired** (turning a mechanic or a module on or off, adding a
command) need a restart, and the reload output says so rather than pretending
otherwise. Message edits and the modules listed as `[OK]` apply immediately.

Your data is **never** deleted by disabling a module. See
[The Module System](../modules/overview.md).

---

## 2. Config Layout (HOCON, not YAML)

Old-school plugins dump everything into one giant `config.yml`. uxmEssentials
splits things up into readable **HOCON `.conf`** files, one concern at a time:

| Path | What lives there |
|------|------------------|
| `config.conf` | Global settings — storage, default language, cross-server, update checks |
| `modules/<module>/config.conf` | One file per module; **`enabled` is the first key** |
| `messages/messages_<lang>.conf` | Player-facing text, per language |
| `commands/commands.conf` | Rename, re-alias or disable any command |
| `menus/*.conf` | Your own custom GUI menus |

A few things that make this pleasant to live with:

- **Keys are kebab-case.** A setting like `pool-size` reads exactly as written.
- **Edits survive updates.** A new version appends the settings it added to the end of
  your file, with the comments that explain them, and touches nothing you wrote. A key
  you deleted on purpose is not put back. See
  [what an update does to your files](../config/overview.md#what-an-update-does-to-your-files).
- **Delete to reset.** Remove a key and it falls back to its default; delete a whole
  file and it regenerates.
- **Atomic reloads.** Config is loaded once and swapped in one go on reload.

<Callout type="tip" title="There is no `modules.conf`">

To enable or disable a module you edit the `enabled` key inside that module's
own `modules/<module>/config.conf` — there is no central switchboard file.

</Callout>

See [Per-Module Config](../config/per-module.md) and
[config.conf (Globals)](../config/global-config.md).

---

## 3. Permissions

Every permission lives under the `uxmessentials.*` prefix, and the defaults are
chosen so a fresh install is sensible with zero setup:

- **Self-service verbs default to `true`** — things players do to their own account
  (`/home`, `/balance`, `/msg`, `/kit`, `/warp`, `/afk`, ...) just work.
- **Staff verbs default to `op`** — mutations and moderation (`/tp` others,
  `/eco`, `/ban`, `/vanish`, item/world verbs, ...).

On top of the plain on/off nodes there are **tiered / quota nodes** with a number
baked into them:

| Node | Meaning |
|------|---------|
| `uxmessentials.home.limit.<n>` | How many homes a player may set |
| `uxmessentials.vault.amount.<n>` | How many vaults a player may own |
| `uxmessentials.tp.cooldown.<seconds>` | Teleport cooldown for that group |
| `uxmessentials.kit.cooldown.<seconds>` | Kit cooldown for that group |

Give `uxmessentials.home.limit.5` and that player can set five homes.

A permission plugin is **optional** — the defaults work on a bare server. If you
run **LuckPerms**, those numbered quotas can also be resolved from LuckPerms *meta*.
See the [Permission Reference](../permissions/reference.md).

---

## 4. Messages & Languages

Every piece of player-facing text is a catalog entry, styled with **MiniMessage**
(the modern `<gradient>`, `<color>`, `<hover>` formatting — never legacy `&`
codes). Text lives per language in `messages/messages_<lang>.conf`, and the plugin
ships with **English (`en`)** and **Turkish (`tr`)** out of the box.

Players pick their own language:

```
/lang            # show your current language
/lang tr         # switch to Turkish
/lang reset      # go back to the server default / your client locale
```

The fallback chain is: your `/lang` choice → your Minecraft client locale → the
server default → the English base. Edit any `.conf` to re-word or re-theme the
plugin without recompiling. See [Messages & Languages](../config/messages.md).

---

## 5. Folia-Ready & Performance

uxmEssentials was built for modern Paper — and **Folia** — from line one. Every bit
of scheduled work goes through Folia-safe schedulers, so heavy jobs (like `/rtp`
safe-location search or a `/baltop` query) run **off the main thread** and hand
control back to the tick loop quickly.

Combined with the modular design — disabled modules doing literally nothing — you
can run a big feature set without paying for the parts you don't use. Your
database, by default, is a **zero-setup embedded SQLite** file. See
[Database](../database/sqlite.md).

---

## Next Steps

- [🧩 The Module System](../modules/overview.md) — the full 23-module list and how to toggle them
- [📟 Commands Overview](../commands/overview.md) — everything you can type in-game
- [⚙️ Configuration Overview](../config/overview.md) — a deeper look at the config tree
- [🔑 Permission Reference](../permissions/reference.md) — every node and quota
