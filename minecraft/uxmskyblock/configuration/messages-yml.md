---
title: messages.yml
order: 740
description: messages.yml holds every player-facing string the plugin sends — chat
  messages, errors, usage hints, status words. Edit it to reword, restyle, or fully
  translate the plugin.
---

---

## Format

Each message is a key with a string value. Standard `&` color codes are supported.

```yaml
visiting: "&7Teleporting you to {player}'s island..."
visit-no-island: "&c'{player}' does not have an island."
visit-locked: "&cThis island is closed to visitors."
island-locked: "&7Your island is now locked (closed to visitors)."
island-unlocked: "&aYour island is now open to visitors."
flag-on: "&aON"
flag-off: "&cOFF"
```

---

## Placeholders

Messages use `{...}` placeholders that the plugin fills in, for example:

| Placeholder | Filled with |
|-------------|-------------|
| `{player}` | A player name |
| `{level}` / `{points}` | Island level / points |
| `{amount}` | A number (bank, etc.) |

The exact placeholders available depend on the message — keep the ones already in
the default string when you reword it.

---

## Status Words

Some short words are reused across menus and messages:

```yaml
flag-on: "&aON"
flag-off: "&cOFF"
visit-open: "&aOPEN"
visit-closed: "&cCLOSED"
```

Changing these updates them everywhere they appear.

---

## Translating

To translate the plugin, you only need to edit:

1. **`messages.yml`** — all chat text
2. **`menus/*.yml`** — menu titles, item names, lore
3. **`config.yml` → `command.subcommands`** — the words players type
4. **`roles.yml`** — role display names

No code changes are ever required.

<Callout type="tip" title="Reload after editing">

Run `/is admin reload` to apply changes without restarting the server.

</Callout>
