---
title: social.conf
order: 645
description: Ratings and the guestbook.
icon: star
---

Ratings and the guestbook: visitors rate an island and leave a line in its guestbook.

The file is `plugins/uxmSkyblock/modules/social.conf`. Every default below is the shipped value.

## `social`

| Key | Default | What it does |
|---|---|---|
| `min-dwell-time` | `30s` | How long a visitor must stay on an island before they may rate it. |
| `prior-weight` | `5` | An island's rating starts as prior-weight votes of prior-mean stars, so a few votes do not make an island the best or the worst at once. Higher prior-weight moves it slower. |
| `prior-mean` | `3.0` |  |
| `max-pinned` | `3` | How many guestbook entries an island may pin to the top. |
| `max-message-length` | `256` | The longest a guestbook entry may be, in letters. |
