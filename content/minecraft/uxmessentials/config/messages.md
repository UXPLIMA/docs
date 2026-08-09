---
title: Messages & Languages
order: 1180
description: Every player-facing line in uxmEssentials — command feedback, GUI titles,
  cooldown notices, everything — comes from a per-language message catalog under plugins/uxmEssentials/messages/.
  No user-visible text is hardcoded, so you can rewrite any line and translate the
  whole plugin.
---

## How to Edit

1. Open the catalog for your language, e.g. `plugins/uxmEssentials/messages/messages_en.conf`.
2. Find the key you want to change and edit its value (the text after `=`).
3. Save.
4. Run `/uxmess reload` — catalogs are swapped atomically on reload.

<Callout type="warning" title="HOCON, not YAML">

Catalogs are HOCON. Each line is `"key" = "value"`. Keep the quotes around both the
key and the value, and keep the `{placeholder}` tokens exactly as they are — they
are filled in by the plugin.

</Callout>

---

## One key per line

Each line maps a **message key** (kebab-case, dot-separated) to a MiniMessage template.
The keys are fixed — they come from the plugin's `MessageKey` catalog and match one-to-one
across every language file. You edit the **values**, never the keys.

```hocon
"command.no-permission" = "<etag:'ERROR'> <bad>You don't have permission.</bad>"
"lang.set" = "<tag:'LANG'> <body>Language set to</body> <value>{locale}</value><body>.</body>"
```

The `{locale}`, `{player}`, `{amount}` and similar tokens are **literal placeholders**
substituted before MiniMessage parses the line. Keep their names exactly; only the text
and styling around them are yours to change.

---

## MiniMessage formatting

Text uses [MiniMessage](https://docs.advntr.dev/minimessage/format.html), Adventure's
tag format. A quick tour of what you can put in a line:

```hocon
"warp.list.header" = "<tag:'WARP'> Warps <muted>(<level>{count}</level>)</muted>:"
"help.entry" = "<muted>-</muted> <click:suggest_command:'/{command}'><hover:show_text:'<muted>Click to type /{command}</muted>'><value>/{command}</value></hover></click>"
```

| Tag | Effect |
|---|---|
| `<red>…</red>`, `<#45cdf9>…</#45cdf9>` | Named or hex colours. |
| `<gradient:#fc7a00:#fcc600>…</gradient>` | A colour gradient across the text. |
| `<bold>`, `<italic>`, `<underlined>` | Text decoration. |
| `<hover:show_text:'…'>…</hover>` | A tooltip shown on mouse-over. |
| `<click:run_command:'/spawn'>…</click>` | Click to run or suggest a command. |
| `<newline>` | A line break (used inside GUI lore). |

uxmEssentials also defines **semantic style tags** — `<value>`, `<body>`, `<money>`,
`<muted>`, `<tag:'…'>`, `<etag:'…'>`, and more — so a line references a role instead of a
raw hex code. The `<prefix>` tag is injected automatically by the plugin; **don't type it
into a line yourself.** These tags and the tone behind them are covered on the
[UI Style](ui-style.md) page.

<Callout type="tip" title="Don't use legacy `&amp;` codes">

MiniMessage replaces the old `&a`/`§a` colour codes and `ChatColor`. Writing `&a` or
a `§` code in a catalog will show up literally, not as a colour. Always use
MiniMessage tags.

</Callout>

---

## Languages that ship

Two catalogs are bundled and kept in perfect key-parity:

| File | Language |
|---|---|
| `messages_en.conf` | English |
| `messages_tr.conf` | Turkish |

Both carry the same key set — only the words differ. To add a language, copy an existing
file to `messages_<code>.conf` (e.g. `messages_de.conf`), translate the **values**, and
leave every key in place.

The fallback language is set once, in the globals file:

```hocon
# config.conf
messages {
  default-locale = "en"
}
```

---

## Which language a player sees

uxmEssentials resolves a player's language through a fallback chain, first match wins:

1. **Their `/lang` override** — a personal choice they set in-game.
2. **Their Minecraft client locale** — if a matching catalog exists.
3. **`default-locale`** from `config.conf`.
4. **The English base**, then the key's own name as a last resort.

Players manage their own override with:

```text
/lang            # show your current language and the available codes
/lang tr         # switch yourself to Turkish
/lang reset      # clear the override and follow your client locale again
```

Under the hood the per-player locale is carried through the whole message pipeline —
including deferred and asynchronous messages — so a line always renders in the requester's
language. The Adventure `GlobalTranslator` handles the translatable-key path so vanilla
translatable components resolve too.

---

## Next Steps

- [UI Style](ui-style.md) — the colour tokens, glyphs and tone the catalogs follow.
- [Config Overview](overview.md) — where the catalogs sit in the file layout.
- [config.conf (Globals)](global-config.md) — setting the `default-locale`.
