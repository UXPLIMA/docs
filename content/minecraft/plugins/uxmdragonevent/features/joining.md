---
title: Joining
order: 33
description: The portal, the command, the entry fee, the IP limit and flight.
icon: door-open
---

Three ways in — a portal, a command, or neither — and several gates in front of them.

## The portal

The intended entrance. Either a WorldGuard region around a portal at spawn, or end portals in
specific worlds. Both are set up in [Setting up the event](../getting-started/setup.md#4-the-portal).

A portal is better than a command for the same reason a door is better than a teleport: everyone
gathers in the same place, it is visible from spawn, and a player who has never heard of the event
can see it happening.

## The command

```yaml
settings:
  join-command:
    enabled: false
    require-permission: true
```

`/dragon join`, off by default. `require-permission: true` gates it behind
`dragonevent.joincommand`, which makes skipping the walk to spawn a rank perk.

## Permission

```yaml
settings:
  permission-required: false
```

With this on, joining at all needs `dragonevent.joinevent` — whichever door they use. This is how you
run the event for a subset of the server.

## Price

```yaml
settings:
  price-required: true
  event-price: 500
  price-required-for-every-join: true
```

Charged through Vault. `price-required-for-every-join: true` charges again when a player who left
comes back, which is what stops the entry fee being a one-off.

An entry fee makes the event a sink rather than a source: money comes out of the economy at the door
and the rewards put some of it back. If your rewards pay more than the fee — as the defaults do —
the event is still net inflation, just less of it.

## Same-IP limit

```yaml
settings:
  same-ip-limit: 0
```

How many players from one address may be in the event at once. `0` disables the check.

This is an alt-account limit, and it is the one worth setting if rewards are worth anything: a
leaderboard where one person occupies three of the top places is not a leaderboard. `1` or `2`
covers most households; be aware that shared connections and VPN users are affected too.

## Leaving and coming back

```yaml
settings:
  keep-stats-on-leave: true
  auto-join-after-leaving: true
```

`keep-stats-on-leave` means a disconnect does not cost a player their damage. `auto-join-after-leaving`
puts them straight back in when they reconnect, without the portal.

Both on is the forgiving configuration and the right default — a server hiccup should not eliminate
someone from an event they paid to enter.

## Leaving the world

A player in the event cannot teleport out. The `can-not-leave-event-world` message tells them to use
`/dragon leave` instead. `dragonevent.teleport.bypass` exempts staff.

## Flight

```yaml
settings:
  auto-flight: false
```

With this on, players holding `dragonevent.fly` get flight automatically on entry. Everyone with the
permission can also toggle it with `/dragon fly`.

Flight is disabled on join and on finish for everyone else; `dragonevent.fly.bypass` exempts a player
from that, which is what you give staff who need to keep flying.

<Callout type="note" title="Flight changes the fight">

A dragon fight where players can fly is a different event — the platform, the pillars and the
approach all stop mattering. Giving flight to a donor rank is a real advantage, not a cosmetic one.

</Callout>
