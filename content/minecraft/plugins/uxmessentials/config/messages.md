---
title: Messages & Languages
order: 1180
description: Editing player-facing text, per language, in MiniMessage.
icon: message-square
---

Every player-facing line comes from a per-language catalog under `plugins/uxmEssentials/messages/`. No
user-visible text is hardcoded, so any line can be rewritten and the whole plugin translated.

## How to Edit

1. Open the catalog for your language, e.g. `plugins/uxmEssentials/messages/messages_en.conf`.
2. Find the key you want to change and edit its value (the text after `=`).
3. Save.
4. Run `/uxmess reload`: catalogs are swapped atomically on reload.

Catalogs are HOCON. Each line is `"key" = "value"`. Keep the quotes around both the key and the value, and
keep the `{placeholder}` tokens exactly as they are; the plugin fills them in.

## One key per line

Each line maps a **message key** (kebab-case, dot-separated) to a MiniMessage template.
The keys are fixed; they come from the plugin's `MessageKey` catalog and match one-to-one
across every language file. You edit the **values**, never the keys.

```hocon
"command.no-permission" = "<etag:'ERROR'> <bad>You don't have permission.</bad>"
"lang.set" = "<tag:'LANG'> <body>Language set to</body> <value>{locale}</value><body>.</body>"
```

The `{locale}`, `{player}`, `{amount}` and similar tokens are **literal placeholders**
substituted before MiniMessage parses the line. Keep their names exactly; only the text
and styling around them are yours to change.

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

uxmEssentials also defines **semantic style tags**: `<value>`, `<body>`, `<money>`,
`<muted>`, `<tag:'…'>`, `<etag:'…'>`, and more, so a line references a role instead of a
raw hex code. The `<prefix>` tag is injected automatically by the plugin; **don't type it
into a line yourself.** These tags and the tone behind them are covered on the
[UI Style](ui-style.md) page.

<Callout type="warning" title="Legacy codes do not work">

MiniMessage replaces the old `&a` and `§a` colour codes. A legacy code written into a catalog shows up
literally instead of colouring the line.

</Callout>

## Languages that ship

Twelve catalogs are bundled, and all of them are written to `plugins/uxmEssentials/messages/` on first
run:

| File | Language | File | Language |
|---|---|---|---|
| `messages_en.conf` | English | `messages_fr.conf` | French |
| `messages_tr.conf` | Turkish | `messages_pl.conf` | Polish |
| `messages_de.conf` | German | `messages_uk.conf` | Ukrainian |
| `messages_ru.conf` | Russian | `messages_zh.conf` | Chinese (Simplified) |
| `messages_es.conf` | Spanish | `messages_ja.conf` | Japanese |
| `messages_pt.conf` | Portuguese (Brazil) | `messages_ko.conf` | Korean |

English is the authoritative key set: every other catalog carries exactly the same keys, and the build
fails if one of them drifts. Each file is yours to edit in place; an update never overwrites a line you
changed, it only appends keys that are new.

### Typography

Catalogs are plain readable text. The small capitals the interface is drawn in are applied when the
language is loaded, not written into the file, so a translator edits ordinary letters and still gets the
house style back. The switch sits at the top of each catalog:

```hocon
"meta.small-caps" = "true"
```

It ships as `true` for English and `false` for every translation. Small capitals exist for the
twenty-six Latin letters only: Russian, Ukrainian, Chinese, Japanese and Korean have no small-capital
form at all, and a language with an accented Latin alphabet would lose its accents halfway through a
word, so a translation reads in ordinary letters. Turn it on for a language of your own if the letters
suit it.

Text a player has to be able to type back (a command, a permission node, a state word standing where a
number usually stands) is wrapped in `<plain>`, which leaves it exactly as written:

```hocon
"skin.usage" = "<tag:'SKIN'> use <value><plain>/skin</plain> a player name</value>."
```

Keep those spans, the `{placeholder}` names and the style tags as English writes them; the prose between
them is what a translation changes.

### Adding your own

To add a language, copy `messages_en.conf` to `messages_<code>.conf` (e.g. `messages_it.conf`), translate
the **values**, and leave every key in place. Set `meta.small-caps` to `false` if your alphabet is not
Latin.

The fallback language is set once, in the globals file:

```hocon
# config.conf
messages {
  default-locale = "en"
}
```

## Which language a player sees

uxmEssentials resolves a player's language through a fallback chain, first match wins:

1. **Their `/lang` override**: a personal choice they set in-game.
2. **Their Minecraft client locale**: if a matching catalog exists.
3. **`default-locale`** from `config.conf`.
4. **The English base**, key by key: a line a translation has not filled in reads in English rather
   than as a raw key name, so a half-finished catalog never shows `warp.list.header` to a player.
5. The key's own name, as a last resort.

Players manage their own override with:

```text
/lang            # show your current language and the available codes
/lang de         # switch yourself to another installed language
/lang reset      # clear the override and follow your client locale again
```

Under the hood the per-player locale is carried through the whole message pipeline,
including deferred and asynchronous messages, so a line always renders in the requester's
language. The Adventure `GlobalTranslator` handles the translatable-key path so vanilla
translatable components resolve too.

Related: [UI Style](ui-style.md), [Config Overview](overview.md), [config.conf (Globals)](global-config.md)
