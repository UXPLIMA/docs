---
title: Community
order: 308
description: Guestbooks, tips, following builders, plot chat and reports.
icon: message-circle
---

## Guestbook

```hocon
guestbook {
  max-length       = 120
  cooldown-seconds = 3600
  keep-per-plot    = 50
  forbidden-words  = []
}
```

`/plot sign <note>` leaves a line on somebody else's plot; nobody signs their own, nor do members. `/plot guestbook` opens
the [book](../menus/community.md#guestbook), where the owner pins one note to the top (it shows on the plot sign) or
removes one. Past `keep-per-plot` the oldest unpinned note makes way. `forbidden-words` is a plain contains, folded to
lowercase; nothing ships.

## Tips

`/plot tip <amount> [currency]` tips the builder of the plot you stand in, between `tips.least` (1) and `tips.most`
(10000). No currency is the first in `currencies.conf`. A builder tipped in items or experience while away is told
rather than paid. `/plot tips` shows what a plot has had.

## Following

```hocon
follow {
  watch-seconds = 300
  max-followed  = 100
  on-published  = ["[message] @effect.follow.published", "[sound] BLOCK_NOTE_BLOCK_BELL 1.0 1.6"]
  on-placed     = ["[message] @effect.follow.placed", "[sound] UI_TOAST_CHALLENGE_COMPLETE 1.0 1.0"]
  on-levelled   = ["[message] @effect.follow.levelled"]
}
```

`/plot follow <player>` hears when a builder publishes a build, places in a contest or levels a plot; `<builder>` and
`<what>` fill the lines. `/plot mute` keeps following without hearing. Nobody offline is told.

## Plot chat

`/plot chat` points your chat at the people standing in the plot you are in, and follows you from plot to plot. A
plot whose `chat` flag is off has no channel, and everybody in it talks to the server as usual.
`uxmplots.chat.spy` reads every plot channel.

## Reports

```hocon
reports {
  max-length       = 120
  cooldown-seconds = 300
  forbidden-words  = []
  reward = ["[message] @effect.report-right", "[give-money] diamonds 4", "[sound] ENTITY_PLAYER_LEVELUP 0.7 1.4"]
}
```

`/plot report <reason>` files one; nobody reports their own plot or one they build on, and one open report per person per
plot. A report decided `valid` or `cleaned` pays the reporter `reward`; `unfounded` pays nobody and punishes nobody. See
[Expiry and staff](expiry-and-staff.md).

Related: [Community windows](../menus/community.md)
