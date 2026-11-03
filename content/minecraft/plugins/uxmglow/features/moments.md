---
title: Moments
order: 403
description: The five moments of a glow and the action lines each runs.
icon: bell
---

The `effects` block of [config.conf](../config/config-conf.md#effects) decides what each moment looks and sounds like.
Every moment is a list, and each line is one thing that happens, in the order written.

| Moment | When it fires | Shipped |
|---|---|---|
| `effects.set` | A solid colour was turned on | `[message] @glow.set`, `[sound] block.amethyst_block_chime 0.6 1.4` |
| `effects.cycling` | A cycle of colours was turned on | `[message] @glow.cycling`, `[sound] block.amethyst_block_chime 0.6 1.4` |
| `effects.cleared` | The glow was turned off | `[message] @glow.cleared`, `[sound] block.amethyst_block_break 0.5 0.9` |
| `effects.restored` | The glow came back at login | `[message] @glow.restored` |
| `effects.lapsed` | The glow did not come back, because the colour is no longer theirs | `[message] @glow.lapsed` |

## The verbs

| Verb | Payload |
|---|---|
| `[message]` | A line to the player |
| `[broadcast]` | A line to everybody |
| `[title]` | Across the middle of the screen |
| `[subtitle]` | `<title> \| <subtitle> <in> <stay> <out>`, the three times in seconds |
| `[actionbar]` | Over the hotbar |
| `[bossbar]` | `<seconds> <colour> <overlay> \| <line>` |
| `[sound]` | `<name> <volume> <pitch>` |
| `[particle]` | `<name> <count> <spread> [data]`; data is a colour `#ff0000`, a block or an item |
| `[console]` | A command run by the server |
| `[player]` | A command run by the player |

Text that starts with `@` names a line of the language file, so the words stay in the language the player reads:
`"[message] @glow.set"`. A brace block says when and how often a line runs: `"[message] {delay=40} <accent>Still
glowing"`, `"[sound] {chance=0.2} ui.button.click 1 1"`.

Delete a moment to keep the shipped default, and write it as `[]` for silence. `/glow off` when nothing glows answers
`glow.already-off` as a plain reply, not a moment.

Related: [config.conf](../config/config-conf.md#effects)
