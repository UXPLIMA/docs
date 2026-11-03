---
title: games.conf
order: 613
description: Kinds, timing, the host on a network, rewards, and the words and trivia files.
icon: gamepad-2
---

`modules/games/games.conf`, read at start and on every reload.

| Key | Default | Meaning |
|---|---|---|
| `kinds` | all seven | `unscramble`, `math`, `trivia`, `first-to-type`, `reaction`, `fill-blank`, `reversed`; empty plays every kind |
| `every-seconds` | `300` | How often a round starts |
| `answer-seconds` | `30` | How long it stays open |
| `min-online` | `2` | No round starts with fewer players online |
| `host` | `true` | This server starts and decides the rounds |
| `host-server` | `""` | The host's `server-name`, on every server |
| `network` | `false` | Each round opens on every server |
| `rewards` | `[give-money] 100`, a message | Action lines for the winner; `{player}`, `{answer}` |

| Folder | Holds |
|---|---|
| `words/<language>.txt` | One word per line; `#` lines skipped |
| `trivia/<language>.conf` | `<id> { question = "...", answers = [ ... ] }` |

`en`, `tr`, `de`, `es`, `fr`, `pt` and `ru` ship in both folders.

Related: [Chat games](../features/games.md)
