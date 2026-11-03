---
title: Messages
order: 609
description: How a line is written, the content words and where they live.
icon: message-square
---

Every line comes from `messages/messages_<language>.conf`. A line is MiniMessage and names roles, not colours; the
colours are in [theme.conf](theme.md). A key that is missing falls back to the default.

The shipped content words are at the end of each file under `content`: `content.skills.<id>.display.name`,
`.description`, `.capstone.badge`; `content.abilities.<id>.display.*`; `content.stats.<id>.display.*`;
`content.resources.<id>.display.*`. A new language is one more messages file, not an edit to every skill.
`/skillsedit read` and `write` reword a line in game.

Related: [Languages](languages.md), [theme.conf](theme.md)
