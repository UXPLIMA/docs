---
title: Converting Other Menus
order: 1120
---

If you are moving to uxmEssentials from another menu plugin, you don't have to rebuild
your GUIs by hand. `/menu convert` reads a competitor's YAML and writes the equivalent
uxmEssentials HOCON into your `menus/` folder, translating materials, names, lore, slots,
click actions and conditions into the [engine's own vocabulary](actions-requirements.md).

It is a **best-effort** tool: expect it to carry roughly three-quarters of a typical
menu across cleanly and to flag the rest for you to finish. It never crashes on a bad
file and it never overwrites your work silently: you review the output and reload when
you're happy.

---

## Supported sources

| Source | Command | Notes |
|---|---|---|
| **DeluxeMenus** | `/menu convert deluxemenus <path>` | The widest coverage: `menu_title`, `size`, `open_requirement`, and each item's material/name/lore/slots and per-gesture click commands and requirements. |
| **zMenu** | `/menu convert zmenu <path>` | Typed action/requirement maps (`- type: …`) mapped onto our refs; a two-line `messages` action becomes two `message:` refs, a `data` action becomes `data-add`/`data-sub`/`data-set`. |
| **OGUI** (OGUI-Custom-GUIs) | `/menu convert ogui <path>` | Reads the single root-key GUI shape; supports both plain `commands:` lists and typed `actions:` (`CONSOLE_COMMAND`, `PLAYER_MESSAGE`, `OPEN_GUI`, …), and open-command aliases. |
| **GUIPlus** | `/menu convert guiplus <path>` | Reads the format its wiki documents (GUIPlus is closed-source). Multi-*scene* files convert their first scene only. |

`<path>` may point at a single file or a directory; a directory converts every file in
it, and one unparsable file never aborts the rest of the batch. Each converted file is
written as `menus/<original-basename>.conf`.

---

## What you get back

Every convert prints a short report:

- **found**: files it looked at,
- **converted**: files it wrote,
- **skipped**: files it could not parse at all,
- **warnings**: the per-menu compromises it made (an unmappable action, a dropped
  condition, a legacy colour code left as-is).

The report never fails the command; read the warnings, then open the new files.

<Callout type="warning" title="Nothing is loaded until you reload">

Convert **writes files but does not auto-reload**: deliberately, so you can review
the output first. Once you're happy, run `/menu reload` to load the new menus, or
`/menu reload <name>` for one.

</Callout>

---

## What maps, and what to watch for

**Carried across cleanly:** title, rows/size, item materials, names and lore, slots,
per-gesture click actions (commands, messages, money, teleport), open requirements and
common conditions (permission, money, world, placeholder comparisons). All of it is
rewritten in uxmEssentials' bare `id:value` form, never the bracketed `[tag]` form the
source used.

**Common caveats:**

- **Colour codes stay legacy.** Old `&a` / `§c` / raw `#77ff77` codes are carried
  through verbatim and warned about once. They still render, but you'll want to move them
  to MiniMessage (`<green>`) to match the rest of your menus.
- **PlaceholderAPI tokens keep working.** `%player_name%` and friends pass straight
  through; our renderer resolves PAPI. OGUI's `{player}` is rewritten to `%player%`.
- **Scripting is never converted.** JavaScript expressions and script actions/conditions
  are dropped by design (a locked decision: the engine has no scripting), with a warning.
- **Unknown actions degrade.** An action type with no clean equivalent becomes a
  best-effort `console:` command when it names one, otherwise it is skipped and warned.
- **Unmappable conditions are dropped.** A condition we have no gate for is removed rather
  than guessed at: check the warnings so a tile isn't left ungated.
- **Multi-page / multi-scene sources lose the extra pages.** GUIPlus converts its first
  scene only; a pattern-backed zMenu item whose pattern file isn't read is skipped whole.
- **Per-action delays and modifiers** (DeluxeMenus `<delay=…>`, GUIPlus `executionDelay`)
  are stripped with a warning, not converted.

<Callout type="tip" title="Review, then reload">

Treat the output as a strong first draft. Skim each new file for warned lines: the
legacy colour codes and the dropped conditions are where the ~20% that didn't map
lives: fix those, then `/menu reload`. Everything else is usually ready to open.

</Callout>

---

## Next Steps

- [Custom Menu Engine](engine.md): edit and extend the files convert produced.
- [Actions & Requirements](actions-requirements.md): the vocabulary convert maps onto.
- [Migrating from EssentialsX](../getting-started/migration.md): the wider data import path.
