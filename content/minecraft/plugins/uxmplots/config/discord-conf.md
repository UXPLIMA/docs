---
title: discord.conf
order: 611
description: The seven webhook events.
icon: message-square
---

Seven events, each with `enabled`, `url` and `message`: `contest-announced`, `judging-opened`, `contest-result`,
`report-filed`, `report-decided`, `plot-claimed`, `plot-sold`. The tokens are in
[Network and Discord](../features/network-and-discord.md#discord).

<Callout type="warning" title="Webhook addresses are secrets">

Anybody holding an address can post into that channel. Keep `discord.conf` out of version control and out of
screenshots.

</Callout>

Related: [Network and Discord](../features/network-and-discord.md)
