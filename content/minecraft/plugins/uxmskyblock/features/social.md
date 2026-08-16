---
title: Coop & Rating
order: 690
description: 'Two social features for islands: temporary coop access and player ratings.'
icon: users
---

## Coop access

Coop lets you grant someone access to your island **without making them a permanent member**. Unlike `/is trust` (which adds a real member and counts against your team limit), a coop player never fills a member slot.

| Command | Aliases | Description |
| --- | --- | --- |
| `/is coop <player>` | `ortak` | Give a player coop access to your island |
| `/is uncoop <player>` | `ortakcikar` | Remove a player's coop access |

- Requires the **INVITE** permission on the island.
- A coop player bypasses the visitor-interaction restriction, so they can use your island the way a member does for interaction.
- Coop is **stored on the island and synced across the network**, so it stays consistent on every backend server.

<Callout type="note" title="Coop vs Trust vs Member">

- **Member** – permanent, counts against the team limit, has a role.
- **Coop** – lightweight, does **not** count against the team limit, no role.
- **Visitor** – anyone else; limited by island flags.

</Callout>

Placeholder: `%skyblock_coop%` returns the number of cooped players.

## Island rating

Visitors can rate islands from **1 to 5 stars**. Ratings live in the shared database, so a score given on one backend is visible on every server in the network.

| Command | Aliases | Description |
| --- | --- | --- |
| `/is rate <1-5>` | `puanla`, `puan`, `oyla` | Rate the island you are standing on |
| `/is rate top` | | Show the rating leaderboard |

- You **cannot** rate your own island.
- Stand on the island you want to rate, then run the command.
- Each player's vote is unique per island; rating again updates your previous vote.

### Configuration

```yaml
rating:
  # How many islands /island rate top lists.
  top-lines: 10
  # An island needs at least this many votes to appear on the leaderboard.
  min-votes: 1
```

### Placeholders

| Placeholder | Returns |
| --- | --- |
| `%skyblock_rating%` | Average rating of your island (e.g. `4.3`) |
| `%skyblock_rating_count%` | Number of votes your island received |
