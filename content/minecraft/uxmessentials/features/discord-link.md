---
title: Discord Link
order: 1430
description: 'Discord Link lets a player bind their Minecraft account to their Discord
  account themselves — no staff ticket, no manual spreadsheet. It''s the foundation
  for Discord-side perks: role sync, verified-member gates, linked-account lookups.
  The flow is deliberately simple: the player asks the game for a one-time code, then
  redeems it with the Discord bot.'
---

<Callout type="info" title="Two pieces, one seam">

The in-game commands live in the main plugin. The **redemption** happens in the
optional `uxmessentials-discord` bridge jar, which runs the Discord bot. The two
talk over a shared service seam — so `/discordlink` issues codes even before the
bridge is installed; they just can't be redeemed until it is.

</Callout>

<Callout type="note" title="Ships switched off">

Linking needs a Discord bot token, so the module does nothing on an install that
has not set one up, and a `/discordlink` a player cannot finish is worse than no
command at all. It therefore ships **disabled**. Set `enabled = true` in
`modules/discordlink/config.conf` and run `/uxmess reload discordlink` once your
bridge is configured.

</Callout>

---

## How Linking Works

```
In-game:  /discordlink
              │  issues a one-time code
              ▼
Discord:  redeem the code with the bot's /link command
              │  bridge confirms the binding
              ▼
Linked:   the accounts are now bound (survives restarts)
```

The player runs `/discordlink`, copies the code they're given, and hands it to the
Discord bot's `/link` command. The bridge confirms it, and the two accounts are
bound. Codes are single-use and expire, so a leaked code is harmless.

---

## The Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/discordlink` | Issue a one-time link code | `uxmessentials.discord.link` |
| `/discordlink status` | Report your current binding | `uxmessentials.discord.link` |
| `/discordlink gui` | Open the link-status panel | `uxmessentials.discord.gui` |
| `/discordunlink` | Remove your Discord binding | `uxmessentials.discord.link` |

---

## Link Outcomes

When the bot redeems a code, the bridge returns one of a fixed set of results, so the
player gets a precise message rather than a vague failure:

| Result | Meaning |
|--------|---------|
| `LINKED` | Success — the accounts are now bound |
| `NO_CODE` | No code was issued / the code is unknown |
| `EXPIRED` | The code was valid but has timed out |
| `ALREADY_LINKED` | That account is already bound |
| `INVALID` | The code is malformed |

---

## What You Need

- The **`uxmessentials-discord`** bridge jar, installed alongside the main plugin.
- A **JDA** configuration with a bot token, in `discord.conf`. The bridge stays
  dormant until it's configured with a valid token.

Once configured, the bridge also mirrors audit and economy notifications into
Discord channels — account linking is one part of a broader Discord integration.

<Callout type="note" title="Without the bridge">

On a bare server with no bridge jar, `/discordlink` and `/discordunlink` still
run and issue codes; there's simply no bot to redeem them against yet. Add the
bridge later and existing codes work as normal — nothing else needs to change.

</Callout>

---

## Next Steps

- [💬 Discord Integration](../integrations/discord.md) — installing and configuring the bridge jar
- [🧩 Per-Module Config](../config/per-module.md) — where module toggles live
- [🔑 Permission Reference](../permissions/reference.md) — the `uxmessentials.discord.*` nodes
