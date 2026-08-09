---
title: Vote
order: 1050
---

## Vote Commands

The `vote` module rewards players for voting for your server on listing sites and builds toward a server-wide **vote party** when the community hits a target. Rewards, streaks, leaderboards, and offline claiming are all handled for you.

<Callout type="info" title="Bridges Votifier">

uxmEssentials listens for standard **Votifier** vote events, so any listing site that already speaks the Votifier/NuVotifier protocol works without extra glue. When a vote lands, the module fires the configured rewards, updates streaks and totals, and advances the vote party.

</Callout>

Votes received while a player is offline are banked and handed out with `/vote claim` the next time they join.

---

## Player Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/vote` | Open the vote-links / vote-sites GUI | `uxmessentials.vote.use` |
| `/vote sites` | Show the configured vote sites | `uxmessentials.vote.use` |
| `/vote claim` | Claim rewards banked while you were offline | `uxmessentials.vote.use` |
| `/vote total [player]` | Lifetime vote total | `uxmessentials.vote.use` |
| `/vote streak [player]` | Current vote streak | `uxmessentials.vote.use` |
| `/vote next` | Time until you can vote again on each site | `uxmessentials.vote.use` |
| `/vote last` | When you last voted | `uxmessentials.vote.use` |
| `/vote top [daily\|weekly\|monthly\|alltime]` | Vote leaderboard for the chosen window | `uxmessentials.vote.top` |
| `/vote remind` | Toggle vote reminders for yourself | `uxmessentials.vote.use` |
| `/vote broadcasts` | Toggle vote broadcasts appearing in your chat | `uxmessentials.vote.use` |
| `/vote testreward` | Simulate a vote to verify your reward setup fires | `uxmessentials.vote.testreward` |
| `/voteparty` | Show progress toward the next vote party | `uxmessentials.voteparty.use` |

---

## Admin Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/vote admin givevote <player> [amount]` | Inject one or more votes for a player | `uxmessentials.vote.admin` |
| `/vote admin reset <player>` | Reset a player's vote data | `uxmessentials.vote.admin` |
| `/voteparty force` | Trigger the vote party immediately | `uxmessentials.voteparty.admin` |
| `/voteparty set <count>` | Set the vote-party counter to an exact value | `uxmessentials.voteparty.admin` |
| `/voteparty add <amount>` | Add to (or, with a negative, subtract from) the counter | `uxmessentials.voteparty.admin` |

<Callout type="tip" title="Test rewards before you go live">

`/vote testreward` runs your reward chain exactly as a real vote would — without needing a listing site to send anything — so you can confirm items, commands, and messages land correctly before players start voting.

</Callout>

---

## Examples

### As a player

```
/vote
/vote claim
/vote streak
/vote top weekly
```

### As an admin

```
/vote testreward
/vote admin givevote Steve 3
/voteparty set 40
/voteparty force
```

---

<Callout type="note" title="Rename or disable any of these">

Every command literal above can be renamed, re-aliased, or switched off in `commands/commands.conf` without touching permissions — see [Renaming Commands](../config/commands-conf.md).

</Callout>

## Next Steps

- [🗳️ Vote Feature Guide](../features/vote.md) - Rewards, streaks, vote parties, and Votifier setup
- [🔐 Permission Reference](../permissions/reference.md) - Every vote and vote-party node
