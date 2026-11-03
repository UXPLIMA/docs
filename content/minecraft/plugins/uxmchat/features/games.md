---
title: Chat games
order: 415
description: Seven kinds of round, timing, rewards, the winners' table, word and trivia files per language, and networks.
icon: gamepad-2
---

Module: `games`. Now and then a round opens in chat, and the first right answer wins.

| Kind | The round |
|---|---|
| `unscramble` | Unscramble a word |
| `math` | Solve a sum |
| `trivia` | Answer a question |
| `first-to-type` | Type a word first |
| `reaction` | Click `[CLICK]` first |
| `fill-blank` | Fill in the missing letters |
| `reversed` | Write a word the right way round |

| Key | Default | Meaning |
|---|---|---|
| `kinds` | all seven | The kinds played; an empty list plays every kind |
| `every-seconds` | `300` | How often a round starts |
| `answer-seconds` | `30` | How long it stays open |
| `min-online` | `2` | No round starts with fewer players online |
| `rewards` | `[give-money] 100` and a message | Action lines for the winner, on their own server; `{player}` and `{answer}` |

The winner's time and streak are said in chat. `/chatgame top` is the winners' table with each player's best
streak; `/chatgame start <kind>` (staff) starts a round now.

## Words and trivia

The words of unscramble, first-to-type, fill-blank and reversed are in `modules/games/words/<language>.txt`, one word
per line; lines starting with `#` are skipped. The trivia bank is `modules/games/trivia/<language>.conf`:

```hocon
creeper {
  question = "Which mob explodes when it gets close?"
  answers = [ "creeper" ]
}
```

A reader gets the list of their language, or English. A question keeps its id in every language's file, and a right
answer in any language wins. A new language is one new file in each folder.

## A network

One server starts and decides the rounds. Set `host = true` on that one and `false` on the rest, and `host-server` to
its `server-name` everywhere. `network = true` opens each round on every server; `false` keeps rounds on the server
that started them.

Related: [games.conf](../config/games-conf.md)
