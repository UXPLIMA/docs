---
title: Animated pack question
order: 403
description: The question a Bedrock player is asked at their first join.
icon: film
---

A Bedrock player is asked once, a few seconds after their first join, how much of the animated pack they want.

| Button | Gives |
|---|---|
| Full | Every frame |
| Light | Every second frame, each shown twice as long |
| None | The first frame only; nothing more is downloaded |

The answer counts from the next join. A player who closes the form is asked again next time; one who answered is not
asked again and changes it in `/bedrock` or with `/bedrock animated`. With `animated-pack = false` under `modules`
nobody is asked.
