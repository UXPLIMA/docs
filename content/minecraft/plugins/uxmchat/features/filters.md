---
title: Filters
order: 405
description: The eleven filters, what each does when it catches a line, the ladder of points, automatic replies and the AI review.
icon: funnel
---

Module: `filters`. The filters read public lines, private messages and mail, and with `filter-surfaces = true` in
`config.conf` signs, books and anvil names too. A player with `uxmchat.bypass.filter` passes them all; staff with
`uxmchat.staff.filter` are told about every catch.

## What a catch does

| Action | Effect |
|---|---|
| `block` | The line does not go out, and the player is told why |
| `censor` | The bad part is starred and the line goes out |
| `shadow` | Only the player sees their line, and is not told |
| `notify` | The line goes out and staff are told |

Every filter also takes `points` (added to the ladder on each catch), `channels` (the channels it reads; empty reads
every channel), `enabled` and `censor-with`.

## The filters

| Filter | Catches | Ships as | Its own keys |
|---|---|---|---|
| `spam` | Too many lines too fast | block, 1 point | `lines = 5` in `window-seconds = 4` |
| `duplicate` | The same line again and again | block, 1 point | `lookback = 3`, `similarity = 0.85`, `allowed` (`gg`, `gl`, `hi`, `ty`, `lol`) |
| `caps` | SHOUTING | censor, 0 points | `min-letters = 6`, `max-ratio = 0.7` |
| `advert` | Links and server addresses, also written as "play dot example dot net" | block, 5 points | |
| `phone` | Phone numbers | block, 2 points | |
| `parrot` | Copying what another player just said | block, 1 point | `min-length = 12`, `similarity = 0.9` |
| `flood` | One character over and over | censor, 0 points | `max-run = 4` |
| `whitespace` | Lines of empty space | censor, 0 points | `max-run = 3` |
| `repeat` | One word over and over | block, 1 point | `max-repeats = 4`, `allowed` (`ha`, `xd`, `gg`) |
| `swear` | Swear words | censor, 3 points | below |
| `unicode` | Invisible, direction, zalgo and full-width characters | censor, 0 points | `categories`, `max-foreign-ratio = 1` |

The `allowlist` holds words and domains no filter ever catches: `class`, `assassin`, `scunthorpe`, `grape`,
`analysis`, and the domains `uxplima.com` and `discord.gg/example`. Put your own server's address there.

## Swear words

The swear filter reads the `words` list, every file in `modules/filters/words/` (one per language, one word per line),
and the files `word-files` names in that folder. It finds a word:

- spelled with numbers, accents or repeated letters, or one letter at a time (`f u c k`);
- backwards, with `reversed = true`;
- spread over the last `split-lines` (3) lines.

A list word of four letters or fewer matches only a whole word, so `ass` is not found in `class`; a longer one is
found inside words too. A word with a Turkish or accented letter, such as `göt`, matches only words written with such
a letter, so `got` passes. `fuzzy = true` also matches a word of seven letters or more with one letter wrong; it ships
off because ordinary words sit one letter away from many list words. `patterns` takes regular expressions.

## The ladder

Points from catches climb a ladder; each rung runs its actions when a player reaches its points. Points fall by
`decay-per-day` (5) each day.

| Points | Shipped action |
|---|---|
| 10 | A warning: the next step is a mute |
| 25 | `[console] mute {player} 10m Too many filter catches` |
| 50 | `[console] mute {player} 1h Too many filter catches` |

`{player}` is the player's name and `{points}` the rung's points. The console `mute` is uxmChat's own or that of
the [punishment plugin](../integrations/punishment-plugins.md) you run.

## Commands as chat

`commands` lists commands whose words pass the filters as if they were chat, such as `/msg` of another plugin:
`tell`, `w`, `whisper`, `r`, `reply`, `me`, `say` ship.

## Automatic replies

Module: `auto-replies`. A line that matches a trigger, a regular expression read without case, gets a reply sent to
the asking player only. `spawn` answers "how do I get to spawn" and `discord` answers "what is the discord". A reply
that starts with `@` is a line of the messages files; `permission` limits who gets it.

## The AI review

Module: `ai`, off by default. After a line goes out, it is sent to a model on OpenRouter with the `rules` you wrote,
and a line the model finds against them adds `points` (5) to the player's ladder; staff read why. Nothing is sent
without an `api-key`, at most `per-minute` (30) lines are reviewed, and lines shorter than `min-length` (4) are not
sent. `uxmchat.bypass.ai` skips it.

<Callout type="warning" title="OpenRouter bills you">

The AI review sends lines to a paid service under your own key. Keep the key out of git and watch the bill.

</Callout>

Related: [filters.conf](../config/filters-conf.md), [Mutes](mutes.md)
