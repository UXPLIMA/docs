---
title: Joining
order: 33
description: The entrances, the checks a player passes on the way in, the entry price, leaving, coming back and flight.
icon: door-open
---

## The entrance

Players join by walking into the WorldGuard region at spawn, or through end portals, depending on
`settings.end-portals.all-end-portals-leads-to-event`. Both are set up in
[Setting up the event](../getting-started/setup.md#3-the-entrance).

A visible entrance at spawn works better than a command alone: players gather in one place, and
someone who has never heard of the event sees it happening.

## The command

```yaml
settings:
  join-command:
    enabled: false
    require-permission: true
```

`/dragon join` works next to the entrance once `enabled` is `true`. With `require-permission: true`
it needs `dragonevent.joincommand`, which makes skipping the walk to spawn a rank perk. Players
without it are told `join-command-no-perm`.

## The checks on the way in

A player is let in only if all of these pass, in this order:

| Check | Message when it fails |
|---|---|
| An event is running | `not-active` |
| `same-ip-limit` is not reached | `same-ip-limit-reached` |
| The event world and its spawn are ready | `can-not-join` |
| `permission-required` is off, or they have `dragonevent.joinevent` | `join-no-perm` |
| They are not already in the event | `already-playing` |
| The entry price can be paid | `economy-not-available` or `not-enough-money` |

A player turned away at an entrance is sent to the lobby spawn. On the way in they are told
`joined-with-price` when a price is set, and `joined-free` otherwise.

## Permission

```yaml
settings:
  permission-required: false
```

With this on, joining needs `dragonevent.joinevent` whichever way the player comes in. This is how
you run the event for part of the server only.

## Price

```yaml
settings:
  price-required: true
  event-price: 500
  price-required-for-every-join: true
```

Charged through Vault. The first time a player enters an event they pay `event-price`. Coming back
after leaving, they pay again only with `price-required-for-every-join: true`.

If Vault or an economy plugin is missing, players with a price to pay are told `economy-not-available`
and stay out. Set `price-required: false` if you run no economy.

The entry price takes money out of the economy and the rewards put money back. If the rewards pay
more than the price, as the defaults do, the event still adds money overall, only less of it.

## Same-IP limit

```yaml
settings:
  same-ip-limit: 0
```

How many players from one address may be in the event at once. `0` turns the check off.

Set it if the rewards are worth anything: a leaderboard where one person holds three of the top
places is not a leaderboard. `1` or `2` covers most households, but players behind a shared
connection or a VPN are affected too.

## Leaving

A player leaves the event when they:

- run `/dragon leave`
- die
- fall into the void
- quit the server

Each one ends their part in the event. `/dragon leave` and the void send the player to the lobby
spawn. A player who died keeps their items only with `end-rules.keepInventory: true`.

While in the event, a player cannot teleport out of the event world: they are told
`can-not-leave-event-world` and have to use `/dragon leave`.

## Coming back

```yaml
settings:
  keep-stats-on-leave: true
  auto-join-after-leaving: true
```

| Setting | Effect |
|---|---|
| `keep-stats-on-leave` | A player who left and comes back to the same event carries on with the damage they had done. Off, they start from zero |
| `auto-join-after-leaving` | A player who quit the server is put back into the event when they log in again, without walking to the entrance. Needs `keep-stats-on-leave` |

Only players in the event when it ends are ranked and rewarded. A player who left and did not come
back gets nothing for the damage they did, even with `keep-stats-on-leave` on.

`auto-join-after-leaving` only covers quitting the server. A player who used `/dragon leave` or died
comes back through the entrance. Coming back costs the price again when
`price-required-for-every-join` is on, including when the plugin puts a player back in by itself.

## Flight

```yaml
settings:
  auto-flight: false
```

| | |
|---|---|
| `auto-flight: true` | Players with `dragonevent.fly` can fly as soon as they arrive |
| `/dragon fly` | Players with `dragonevent.fly` turn flight on and off themselves |
| Flight from other plugins | Taken away on arrival, unless the player has `dragonevent.fly` or `dragonevent.fly.bypass` |
| Leaving or the event ending | Flight is turned off, unless the player has `dragonevent.fly.bypass` |

<Callout type="note" title="Flight changes the fight">

A flying player reaches the dragon whenever they want and never falls, so the platform, the pillars
and the approach stop mattering. If the leaderboard pays out well, giving flight to a donor rank
gives them the leaderboard.

</Callout>
