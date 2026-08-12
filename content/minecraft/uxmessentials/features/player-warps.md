---
title: Player Warps
order: 1220
---

Player warps let ordinary players publish their own destinations for others to visit: a
shop, a farm, a build worth showing off. They sit between [homes](../modules/homes.md) (fully
private) and [server warps](warps.md) (staff-owned, server-wide). A player warp is
**owned by the player who created it**, counts against that player's personal quota, and
carries its own access rules, price, category, rating and staff list.

Two players can each own a warp called `shop` without clashing, so callers disambiguate
with the owner argument. Everything is **DB-backed**, so warps survive world rollbacks
and restarts. The roots are `/pwarp`, `/setpwarp` for creation and `/pwarps` for listing.

---

## Creating and visiting

| Action | Command | Permission |
|--------|---------|------------|
| Create / re-anchor at your position | `/setpwarp <name>` | `uxmessentials.pwarp.set` |
| Visit | `/pwarp <name> [password]` | `uxmessentials.pwarp.use` |
| Open the browse GUI | `/pwarp` | `uxmessentials.pwarp.use` |
| List warps | `/pwarps [player]` | `uxmessentials.pwarp.list` |
| Delete one of yours | `/pwarp del <name>` | `uxmessentials.pwarp.delete` |
| Move one of yours here | `/pwarp move <name>` | `uxmessentials.pwarp.move` |
| Rename one | `/pwarp rename <name> <newName>` | `uxmessentials.pwarp.rename` |
| Show a warp's card | `/pwarp info <name>` | `uxmessentials.pwarp.info` |

If the warp has a password, append it: `/pwarp shop hunter2`.

---

## The GUIs

`/pwarp` with no arguments opens the **browse grid** directly, not a hub: one page of
warps you can actually use, with a clean empty state when the server has none. It is
paged, and it remembers your page, sort and filters while it is open.

From the browse grid's own control you reach the **categories landing**, which holds:

- one button per category, drilling into the browse filtered to that category;
- quick scopes: browse all, **my warps**, **favourites**, **top rated**;
- the **sponsor showcase** band at the bottom (empty when sponsorship is off).

Categories ship with a default set out of the box (Building, Shop, PvP, Farm, Redstone,
Misc) and swap over to your own definitions once you define any.

Clicking a warp opens its **view** card (rate it, favourite it, teleport). A warp you may
manage also offers the **manage** menu, which fans out to the **members**, **whitelist**
and **bans** screens and the icon picker. Every screen is a file under
`modules/playerwarps/gui/`, so all of it is re-themeable:

| File | Screen |
|---|---|
| `pwarp-browse.conf` | The paged warp grid `/pwarp` opens |
| `pwarp-categories.conf` | The categories landing with quick scopes and sponsors |
| `pwarp-view.conf` | One warp's card |
| `pwarp-rate.conf` | The star picker |
| `pwarp-manage.conf` | The owner's management menu |
| `pwarp-editor.conf` | The property editor behind `/pwarp edit` |
| `playerwarp-list.conf` | The list geometry `/pwarps` renders with |
| `pwarp-members.conf`, `pwarp-whitelist.conf`, `pwarp-bans.conf`, `pwarp-icon.conf` | The people and icon screens |

---

## Access, visibility and password

There are **two independent axes**. Visibility decides whether the warp is *listed*;
access decides who may *enter*.

```
/pwarp visibility public|private <name>          # listed or hidden
/pwarp access <name> PUBLIC|PASSWORD|WHITELIST|PRIVATE
/pwarp password <name> <password>                # set
/pwarp password <name> clear                     # remove
```

| Access | Who gets in |
|---|---|
| `PUBLIC` | Anyone |
| `PASSWORD` | Anyone who types the password |
| `WHITELIST` | Only players on the warp's whitelist |
| `PRIVATE` | Only the owner and the warp's staff |

Passwords are **hashed with PBKDF2**; nothing stores or logs the plaintext, so a password
can be verified but never read back, not even by an operator.

The gate runs in a fixed order on every visit: ban check, then access (whitelist or
password), then price, then safe-landing. Each stage has its own bypass node (below).

---

## People: members, whitelist and bans

A warp can be run by more than one person. Roles are hierarchical, and the capability
matrix is enforced inside the use case, not by permissions:

| Role | May do |
|---|---|
| **Owner** | Everything |
| **Co-owner** | Everything except delete, transfer, manage members and sponsor |
| **Manager** | Edit the warp's metadata, manage the whitelist, manage bans |

```
/pwarp members <name> add <player> co-owner|manager
/pwarp members <name> remove <player>
/pwarp whitelist <name> add|remove <player>
/pwarp ban <name> <player> [duration] [reason]     # 7d, 12h, or omit for permanent
/pwarp unban <name> <player>
/pwarp transfer <name> <player>                    # hand over ownership
```

An out-of-range duration token is folded into the reason rather than rejected, so
`/pwarp ban shop Steve being rude` bans permanently with that reason.

---

## Money: price, earnings and withdrawal

A warp can charge for entry. The fee is split between the server and the owner:

```
/pwarp price <name> <amount> [currency]     # 0 makes it free again
/pwarp withdraw <name>                      # pay the accrued earnings out to the owner
```

```hocon
payout {
  cut-percent = 10      # the server's share of each entry fee; the rest banks to the owner
  auto-payout = false   # settle the owner immediately, or bank it for /pwarp withdraw
}
```

`auto-payout` only works for currencies that can be written to an offline player.
Otherwise the takings **bank on the warp** and the owner collects with `/pwarp withdraw`.
Everything here needs the [economy](../modules/economy.md) module; without a provider a priced warp
simply cannot be charged for.

---

## Ratings and favourites

Visitors rate a warp one to five stars, and star it as a favourite for their own quick
scope:

```
/pwarp rate <name> \<1-5>
/pwarp favourite <name>
/pwarp unfavourite <name>
```

The "top rated" browse does **not** sort on the raw average. It uses Bayesian smoothing,
so a single five-star vote cannot outrank a warp with a hundred good ones:

```hocon
ratings {
  confidence = 10   # how many global-mean votes of prior each warp starts with
}
```

Raise `confidence` to pull new warps harder toward the server average; lower it to let a
young warp climb faster.

### Rating rewards

Off by default. When on, rating pays out, deduped so it cannot be farmed: **a rater is
rewarded once per warp** (re-rating the same warp grants nothing) and **an owner is
rewarded once per unique rater**.

```hocon
ratings.rewards {
  enabled = false
  rater { money = 0, currency = "default", command = "" }
  owner { money = 0, currency = "default", command = "" }
}
```

A money reward needs an economy provider; a command reward always works and runs from the
console with `%player%` replaced by the rewarded player's name. Leave one side blank to
reward only the other.

---

## Rent: paying to stay listed

Off by default. When on, every warp pays rent each period or falls out of the list. The
sweep charges the **warp's own earnings bank first**, then the owner's wallet.

```hocon
rent {
  enabled = false
  amount = 100
  currency = "default"
  period-days = 7
  grace-days = 3                    # a suspended warp waits this long before being archived
  hard-delete-after-days = -1       # -1 = an archived warp is never hard-deleted
  check-interval-minutes = 60
  reminder-hours = [24, 12, 6, 1]   # mail the owner this long before the term lapses
  exempt {
    players = []
    categories = []
    worlds = []
  }
}
```

The lifecycle is **live → suspended → archived**, never deleted. A suspended warp is
unlisted and unusable but recoverable; after `grace-days` it is archived, and an operator
restores it with `/pwarp admin restore <id>`. Owners get a mail at each `reminder-hours`
window before the term lapses.

<Callout type="warning" title="Turning rent on affects every existing warp">

A warp with no rent term is exempt only while rent is off. Switch it on and every
warp starts a term, so announce it before you do, or use the `exempt` lists.

</Callout>

---

## Sponsorship: paid pinned slots

Off by default. An owner buys a time-limited pinned slot in the browse and the categories
showcase.

```
/pwarp sponsor <name> [days]
```

```hocon
sponsor {
  enabled = false
  slots = 5                       # a purchase takes the lowest free one
  duration-days = 7               # the longest term one purchase may run
  price = 1000
  currency = "default"
  max-concurrent-per-player = 1
  cooldown-days = 3               # a warp waits this long after expiry before it can be sponsored again
  check-interval-minutes = 60
}
```

Omitting `days` buys the configured default term, and the use case clamps any request to
`duration-days`. A sponsored warp **cannot be transferred** while its term is live. When
a term expires the sweep frees the slot and stamps the cooldown. The whole sub-group needs
an economy provider: a paid slot cannot be sold for free.

---

## Cross-server warps

Off by default. With a proxy (BungeeCord or Velocity) in front, a shared database, and a
unique `network.server-id` per backend, a warp on another server is reachable from here.

```hocon
cross-server {
  enabled = false
  arrival-delay-ticks = 20      # settle time after the target-server join before the local hop
  request-ttl-seconds = 30      # a pending teleport older than this is refunded, not honoured
}
```

Using a remote warp charges on **this** backend, records the intent in the shared
`player_warp_pending_teleports` table, and connects the player across the proxy; the
target backend completes the teleport on join. If the warp has gone by then, the player
is refunded. With no proxy channel registered a remote warp is **refused and refunded**
rather than silently hopped locally.

See [Cross-Server](../cross-server/overview.md) for the network setup.

---

## Quotas: how many warps per player

```
uxmessentials.pwarp.limit.<n>
uxmessentials.pwarp.limit.<n>.<world>     # optional per-world scope
```

The highest matching `<n>` wins. Without any node the module config decides:

```hocon
default-limit = 3
```

`/setpwarp` refuses once a player is at their limit, so no route can exceed the quota.

---

## Operator commands

Every admin verb works on **any** warp by its surrogate id (shown by `/pwarp info` and in
the GUI), gated by the single `uxmessentials.pwarp.admin` node:

| Command | What it does |
|---|---|
| `/pwarp admin restore <id>` | Bring an archived warp back |
| `/pwarp admin purge <id>` | Warn, then require `confirm` |
| `/pwarp admin purge <id> confirm` | Irreversibly delete it |
| `/pwarp admin delete <id> [confirm]` | The same, under the other name |
| `/pwarp admin setowner <id> <player>` | Re-home a warp to another owner |
| `/pwarp admin reload` | Re-read the module's config |

---

## Player warps vs server warps

| | Player warps | Server warps |
|---|---|---|
| Created by | Any player (quota-limited) | Staff |
| Ownership | The creating player, with co-owners and managers | The server |
| Access | Public / password / whitelist / private, plus bans | Permission and password |
| Money | Entry price banks to the owner, optional rent and sponsorship | Fixed cost to the server |
| Discovery | Categories, ratings, favourites, sponsored slots | A flat list |
| Counts against a quota | Yes (`pwarp.limit.<n>`) | No |
| Command root | `/pwarp`, `/setpwarp`, `/pwarps` | `/warp` |

Use server warps for infrastructure (spawn, arenas, markets), player warps for
player-generated content (shops, showcases, community farms).

---

## Permissions

Self-service nodes default `true`; the two operator nodes and every bypass default `op`.
Holding a node lets a player **run** the verb; whether they may run it on a *particular*
warp is still decided by the role matrix above.

| Node | Verb |
|------|------|
| `uxmessentials.pwarp.use` | Visit warps, open the browse GUI |
| `uxmessentials.pwarp.set` | `/setpwarp` |
| `uxmessentials.pwarp.delete` | `/pwarp del` |
| `uxmessentials.pwarp.list` | `/pwarps` |
| `uxmessentials.pwarp.public` | `/pwarp visibility` |
| `uxmessentials.pwarp.move` | `/pwarp move` |
| `uxmessentials.pwarp.rename` | `/pwarp rename` |
| `uxmessentials.pwarp.displayname` | `/pwarp displayname` |
| `uxmessentials.pwarp.description` | `/pwarp description` |
| `uxmessentials.pwarp.icon` | `/pwarp icon` |
| `uxmessentials.pwarp.category` | `/pwarp category` |
| `uxmessentials.pwarp.access` | `/pwarp access` |
| `uxmessentials.pwarp.password` | `/pwarp password` |
| `uxmessentials.pwarp.price` | `/pwarp price` |
| `uxmessentials.pwarp.info` | `/pwarp info` |
| `uxmessentials.pwarp.favourite` | `/pwarp favourite`, `/pwarp unfavourite` |
| `uxmessentials.pwarp.rate` | `/pwarp rate` |
| `uxmessentials.pwarp.transfer` | `/pwarp transfer` |
| `uxmessentials.pwarp.sponsor` | `/pwarp sponsor` (also needs the sub-group on) |
| `uxmessentials.pwarp.withdraw` | `/pwarp withdraw` |
| `uxmessentials.pwarp.members` | `/pwarp members` |
| `uxmessentials.pwarp.ban` | `/pwarp ban`, `/pwarp unban` |
| `uxmessentials.pwarp.whitelist` | `/pwarp whitelist` |
| `uxmessentials.pwarp.limit.<n>` | The per-player quota |

| Operator / bypass node | Effect | Default |
|---|---|---|
| `uxmessentials.pwarp.admin` | The whole `/pwarp admin` group | `op` |
| `uxmessentials.pwarp.gui` | Manage **every** player's warps in the GUI (without it you edit only your own) | `op` |
| `uxmessentials.pwarp.bypass.ban` | Enter a warp you are banned from | `op` |
| `uxmessentials.pwarp.bypass.password` | Enter without the password | `op` |
| `uxmessentials.pwarp.bypass.whitelist` | Enter without being whitelisted | `op` |
| `uxmessentials.pwarp.bypass.safety` | Use a warp whose destination is unsafe | `op` |
| `uxmessentials.pwarp.bypass.cost` | Use a priced warp without paying | `op` |
| `uxmessentials.module.playerwarps` | Reload / inspect the module | `op` |

---

## Tips and gotchas

- **Name collisions are by design.** Two players can both own `shop`; bare
  `/pwarp shop` always means yours.
- **Visibility is not access.** A private-visibility warp is unlisted; a `PRIVATE`-access
  warp is closed. You can have a listed warp nobody may enter, and an unlisted warp that
  anyone with the name may.
- **Rent and sponsorship are economy features.** Both stay inert without an economy
  provider, so switching them on with no economy module does nothing at all.
- **Archived is not deleted.** Only `/pwarp admin purge … confirm` destroys a warp.
- **The GUI gate widens, it does not grant.** Without `uxmessentials.pwarp.gui` a player
  still opens the GUI, just scoped to their own warps.

---

## Next Steps

- [🏠 Homes & Warps Commands](../commands/homes-warps.md) - The full command reference
- [📍 Warps](warps.md) - The staff-owned, server-wide counterpart
- [🏠 Homes](../modules/homes.md) - The fully-private counterpart
- [💰 Economy](../modules/economy.md) - What entry fees, rent and sponsorship charge through
- [🔐 Permission Reference](../permissions/reference.md) - Quota tiers and bypass nodes
