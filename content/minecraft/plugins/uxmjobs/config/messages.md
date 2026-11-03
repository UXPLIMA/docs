---
title: Messages
order: 613
description: How a line is written, the prefixes, the blocks of keys, and the content words.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default the code carries.

| Prefix | Draws |
|---|---|
| `<tag:'JOBS'>` | The plugin's category word |
| `<etag:'ERROR'>` | An error |
| `<etag:'BOUND'>`, `<etag:'ALERT'>` | The preview's bound line, an income alert |
| `<h:'...'>` | A header |

| Block | Said when |
|---|---|
| `admin.*` | The operator commands, the doctor |
| `job.*` | Joining, leaving, level ups, `mine`, `browse`, `stats`, `points` |
| `top.*`, `board.*`, `period.*` | `/jobs top` |
| `placeholder.*` | Words the board placeholders answer with: `nobody`, `none`, `resets` |
| `pay.*` | The action bar and boss bar lines, their toggles, the empty account |
| `why.*` | `/jobs why` |
| `booster.*`, `preview.*`, `report.*` | Those commands |
| `menu.*` | Every window's titles, tiles and buttons |
| `contract.*` | `/contracts` and the shipped contracts' promotion lines |
| `shop.*` | Buying, and the shipped lines' `shop.done.*` |
| `chat.format`, `chat.separator` | The chat prefix |
| `requirement.*`, `language.*`, `command.*` | Refused requirements, languages, the command layer |
| `zone.*`, `sign.*`, `dialog.*`, `editor.*` | Those features |
| `words.*` | How a ledger verdict and a protection reason read in `/jobs why` |
| `reward.*` | The shipped rewards' lines |

## Content words

The shipped content files name their words with `@content.<file>.<path>`: `content.jobs.miner.name`,
`content.contracts.evening-shift.description`, `content.shop.lines.quiver.name`, `content.titles.titles.default.50`.

Related: [Languages](languages.md), [theme.conf](theme.md)
