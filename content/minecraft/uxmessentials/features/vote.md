---
title: Vote
order: 1420
description: 'The vote module turns votes on server-listing sites into in-game rewards.
  It bridges NuVotifier / Votifier: when a listing confirms a vote, uxmEssentials
  receives it, pays out the configured rewards, advances the server-wide vote party, and
  updates the leaderboards. Players who vote while offline don''t lose out; their
  rewards wait to be claimed. Everything is configured in vote.conf.'
---

<Callout type="note" title="Votifier is the delivery mechanism">

The listing sites send their confirmation through **NuVotifier/Votifier**. If
that plugin isn't installed, the vote listener simply stays dormant:
`/vote`, `/vote top` and `/voteparty` still work, only the incoming
site-triggered rewards are inactive until you add it.

</Callout>

<Callout type="info" title="Ships switched off">

Because the whole feature waits on Votifier and on the server being listed
somewhere, a fresh install keeps it **disabled** rather than offering a `/vote`
that leads nowhere. Set `enabled = true` in `modules/vote/config.conf` and run
`/uxmess reload vote` once your listings are set up.

</Callout>

---

## How a Vote Flows

```
Player votes on a listing site
        │
        ▼
NuVotifier confirms the vote → uxmEssentials
        │
        ├── pays the reward (now, or holds it for offline claim)
        ├── advances the vote-party counter
        └── updates totals, streaks and leaderboards
```

**Which account gets credited.** Listing sites send back whatever the voter typed on
their form, so the case often differs from the in-game name. The vote is matched against
the accounts the server already knows, in any case, so a vote sent as `griefer` credits
`Griefer`. This holds on offline-mode (cracked) servers, where the server itself cannot
translate a name whose case differs; see
[Offline Mode](../getting-started/offline-mode.md). A vote for a name nobody has ever
joined under is still kept, waiting for that player's first login.

---

## Vote Sites

`/vote` (and `/vote sites`) shows the player where to vote: a chat list or a
**vote-sites GUI** of clickable links. The sites and their URLs come from
`vote.conf` (`vote-links[]`, `sites[]`), and `default-cooldown-minutes` sets how
often each site can be voted.

---

## Rewards

Rewards are defined in `vote.conf` under `rewards {}`, with several layers that stack:

| Layer | When it fires |
|-------|---------------|
| `per-vote[]` | Every single vote |
| `per-site{}` | Bonus tied to a specific listing site |
| `first-vote[]` | The player's very first vote ever |
| `milestones[]` | Reaching a lifetime-total threshold |

Rewards are paid immediately when the player is online. If they voted **offline**,
the reward is held and paid on their next login, or claimed on demand:

- `/vote claim`: collect any rewards waiting for you.
- `claim { auto }` in config pays them automatically on join instead.
- `offline-vote-limit` caps how many offline votes bank up.

---

## Streaks & Totals

Every vote is counted. Players check their standing with:

| Command | Shows |
|---------|-------|
| `/vote total [player]` | Lifetime vote count |
| `/vote streak [player]` | Current consecutive-day streak |
| `/vote next` / `/vote last` | When you can vote again / when you last voted |

Streaks reward loyalty: vote every day and the streak climbs; miss a day and it
resets. Milestone rewards read off the lifetime total.

---

## Vote Party

A vote party is a server-wide goal: once the community casts enough votes, everyone
gets a payout. `/voteparty` shows progress toward the next party; the threshold is
set in `vote.conf`. Staff can nudge the counter:

| Command | Effect |
|---------|--------|
| `/voteparty force` | Trigger the party now |
| `/voteparty set <count>` | Set the counter to a value |
| `/voteparty add <amount>` | Add to the counter |

---

## Leaderboards

`/vote top [daily | weekly | monthly | alltime]` ranks the top voters over the chosen
window. The daily/weekly/monthly boards reset on their cycle; `alltime` never does.
These pair naturally with a [leaderboard hologram](holograms.md) or a
[HUD](hud.md) line.

---

## Reminders & Broadcasts

- `/vote remind`: toggle personal reminders that nudge you when you can vote again.
- `/vote broadcasts`: toggle whether you see the server's vote broadcasts.
- Server-side reminder/broadcast behaviour is set in `vote.conf`
  (`reminders {}`, off by default), including an optional Discord `webhook-url`.

---

## Testing & Admin

| Command | Effect | Permission |
|---------|--------|------------|
| `/vote testreward` | Simulate a vote to verify your reward config | `uxmessentials.vote.testreward` |
| `/vote admin givevote <player> [amount]` | Inject votes for a player | `uxmessentials.vote.admin` |
| `/vote admin reset <player>` | Reset a player's vote data | `uxmessentials.vote.admin` |

<Callout type="tip" title="Always `/vote testreward` after editing rewards">

A test vote runs your full reward pipeline against yourself so you can see
exactly what a real vote pays out, before a player finds a mistake.

</Callout>

---

## Next Steps

- [🗳️ Vote Commands](../commands/vote.md): the full `/vote` and `/voteparty` reference
- [🧩 Per-Module Config](../config/per-module.md): `vote.conf` sites, rewards, party threshold
- [🪧 Holograms](holograms.md): build a live vote leaderboard
- [🔑 Permission Reference](../permissions/reference.md): the `uxmessentials.vote.*` nodes
