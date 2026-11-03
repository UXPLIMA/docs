---
title: filters.conf
order: 606
description: The allowlist, every filter and its keys, the ladder, automatic replies, commands read as chat, and the word files.
icon: funnel
---

`modules/filters/filters.conf`, read at start and on every reload.

## The keys every filter takes

| Key | Meaning |
|---|---|
| `enabled` | `false` turns it off |
| `action` | `block`, `censor`, `shadow` or `notify` |
| `points` | Added to the player's ladder on each catch |
| `channels` | The channels it reads; empty reads every channel |
| `censor-with` | The character a censored part is written with |

## allowlist

```hocon
allowlist {
  words = [ "class", "assassin", "scunthorpe", "grape", "analysis" ]
  domains = [ "uxplima.com", "discord.gg/example" ]
}
```

## Each filter

| Filter | Keys and defaults |
|---|---|
| `spam` | block, 1; `lines = 5`, `window-seconds = 4` |
| `duplicate` | block, 1; `lookback = 3`, `similarity = 0.85`, `allowed = [gg, gl, hi, ty, lol]` |
| `caps` | censor, 0; `min-letters = 6`, `max-ratio = 0.7` |
| `advert` | block, 5 |
| `phone` | block, 2 |
| `parrot` | block, 1; `min-length = 12`, `similarity = 0.9` |
| `flood` | censor, 0; `max-run = 4` |
| `whitespace` | censor, 0; `max-run = 3` |
| `repeat` | block, 1; `max-repeats = 4`, `allowed = [ha, xd, gg]` |
| `swear` | censor, 3; `censor-with = "*"`, `words = []`, `word-files = []`, `patterns = []`, `fuzzy = false`, `reversed = true`, `split-lines = 3` |
| `unicode` | censor, 0; `categories = [invisible, direction, zalgo, full-width]`, `max-foreign-ratio = 1` |

`similarity` 1.0 is the very same line. A `unicode` line whose share of letters from other scripts is over
`max-foreign-ratio` is caught too; 1 never catches that.

## ladder

```hocon
ladder {
  decay-per-day = 5
  rungs = [
    { points = 10, actions = [ "[message] <warn>Your lines keep tripping the filter. The next step is a mute." ] }
    { points = 25, actions = [ "[console] mute {player} 10m Too many filter catches" ] }
    { points = 50, actions = [ "[console] mute {player} 1h Too many filter catches" ] }
  ]
}
```

## auto-replies

```hocon
auto-replies {
  spawn {
    triggers = [ "how (do|can) i (get|go) (to )?spawn", "where is spawn" ]
    reply = "@auto-reply.spawn"
    permission = ""
  }
}
```

## commands

`commands = [ "tell", "w", "whisper", "r", "reply", "me", "say" ]`: commands whose words pass the filters as if they
were chat.

## Word files

`modules/filters/words/<language>.txt`, one word per line: `en`, `tr`, `de`, `es`, `fr`, `pt` and `ru` ship. Every file
is read whatever language a player reads. A new language is one new file.

Related: [Filters](../features/filters.md)
