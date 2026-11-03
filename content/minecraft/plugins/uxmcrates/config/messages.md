---
title: Messages
order: 506
description: How a line is written, the roles and prefixes it uses, and every block of keys with the values each fills in.
icon: message-square
---

Every line the plugin writes comes from `messages/messages_<language>.conf`, one file per language. No text is written
in the code, so any line can be rewritten and the plugin translated whole. Run `/uxmcrates reload` after an edit.

## How a line is written

```hocon
crate {
  won = "<tag:'CRATES'> <body>You won <value><caps><reward></caps><body>."
}
```

- The file is HOCON blocks: `crate.won` is the key `won` inside the block `crate`.
- A line is [MiniMessage](https://docs.advntr.dev/minimessage/format.html). Legacy `&a` codes are shown as written.
- A line names a **role**, not a colour: `<body>` is the text, `<value>` a name or a number, `<good>`, `<bad>`,
  `<warn>`, `<muted>`, `<subtext>`, `<accent>` and `<cta>` the rest. The colours are in [theme.conf](theme.md).
- A tag that is not a role is a **value** the plugin fills in, such as `<crate>`. A value is inserted as text.
- `<caps>...</caps>` draws a name in small capitals, for the languages [theme.conf](theme.md#small-capitals) turns
  them on for.
- A line you delete falls back to the default built into the plugin.

## The prefixes

| Tag | Draws |
|---|---|
| `<tag:'CRATES'>` | The category word and the separator glyph, in the category's colour |
| `<etag:'ERROR'>` | The same for an error, in the `bad` colour |
| `<h:'...'>` | A header, in the `header` gradient |

## The keys

| Block | Values | Said when |
|---|---|---|
| `placeholder.nobody`, `none`, `resets.*` | `<days>`, `<hours>`, `<minutes>` | The board placeholders |
| `admin.usage`, `version`, `reloaded`, `reload-failed` | `<version>`, `<ms>` | The bare root, `version`, `reload` |
| `admin.doctor.*` | `<check>`, `<said>` | `/uxmcrates doctor` |
| `error.player-only`, `unknown-player` | `<player>` | Shared refusals |
| `language.*` | `<language>`, `<languages>`, `<player>`, `<input>` | `/uxmcrates lang` |
| `command.*` | `<input>`, `<argument>`, `<allowed>`, `<time>`, `<command>`, `<page>` | The command layer: help pages, a rejected argument |
| `crate.unknown`, `no-key`, `no-permission`, `requirements`, `cannot-pay`, `protected-land`, `on-cooldown`, `once-only`, `nothing-to-give`, `already-opening`, `preview-too-fast` | `<crate>`, `<amount>`, `<key>`, `<time>` | A refused opening |
| `crate.won`, `forced`, `broadcast` | `<reward>`, `<player>`, `<crate>` | A win, a forced opening, a win broadcast |
| `pending.*` | `<reward>`, `<count>` | A reward stored, claimed or waiting at join |
| `key.*` | `<key>`, `<amount>`, `<player>`, `<count>`, `<when>`, `<movement>`, `<source>` | Key commands, sending, the ledger |
| `place.*` | `<crate>`, `<position>` | Placing, removing, the link tool |
| `envoy.drop.*` | | The shipped envoy's lines |
| `envoy.list-*`, `state-*`, `next-never`, `started`, `stopped`, `already-running`, `not-running`, `unknown` | `<envoy>`, `<state>`, `<next>` | Envoy commands |
| `open.wrong-world` | | A crate opened in a world it does not allow |
| `pick.nothing-taken` | | A pick window closed with nothing taken |
| `reset.*` | `<crate>`, `<player>` | Resets |
| `reroll.*` | `<player>`, `<amount>`, `<reward>` | Rerolls given, used, kept or refused |
| `milestone.*` | `<openings>`, `<crate>` | A rung reached or taken |
| `log.*` | `<when>`, `<player>`, `<reward>`, `<chance>`, `<filter>` | `/uxmcrates log` in the console |
| `migrate.*` | `<plugin>`, `<crates>`, `<keys>`, `<count>`, `<file>` | `/uxmcrates migrate` |
| `editor.*` | `<crate>`, `<key>`, `<reward>`, `<input>` | The editor's replies |
| `menu.title.*`, `menu.button.*` | `<crate>` | Window titles and the page and back buttons |
| `menu.crate`, `reward`, `key`, `pending`, `cost`, `reroll`, `bulk`, `milestone`, `log` | `<chance>`, `<cap>` | The tiles of each window |
| `menu.key-editor.*`, `menu.editor.*` | | The editor's buttons |
| `time.*` | `<days>`, `<hours>`, `<minutes>`, `<seconds>` | A duration, built from the two largest units |
| `words.movement.*` | | `granted`, `spent`, `transferred`, `set to a number` in the ledger |
| `content.*` | | The words the shipped crates, keys, rewards, envoys, milestones and link tool name with `@` |

## Words of your own content

A crate, key, reward, rarity, group, milestone, envoy or reroll refusal you add can name its words with `@`: write
`name = "@content.crates.vote.name"` and add `content.crates.vote.name` to every messages file. Or write a block per
language in the file itself, or one text for everyone. See [Concepts](../getting-started/concepts.md#a-crate-is-a-file).

Related: [Languages](languages.md), [theme.conf](theme.md)
