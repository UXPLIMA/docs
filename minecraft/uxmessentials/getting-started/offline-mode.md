---
title: Offline Mode (Cracked)
order: 930
description: uxmEssentials runs fully on offline-mode servers (online-mode=false in
  server.properties, often called cracked). Every module works the same; nothing is
  gated behind a premium account check. This page explains the one place where an
  offline-mode server behaves differently from an online-mode one, and what the plugin
  does about it.
---

---

## How Accounts Are Identified

Every piece of player data in uxmEssentials (homes, balance, vaults, punishments, kits)
is keyed by **UUID**, never by name. That is what makes a rename harmless and a lookup
exact.

Where the UUID comes from depends on the server mode:

| Mode | Where the UUID comes from |
|------|---------------------------|
| **Online** | Mojang, one stable UUID per account, whatever the name is today |
| **Offline** | Derived from the name itself, by the server, character for character |
| **Behind a proxy** | Whatever Velocity or BungeeCord forwards, so the backends agree |

On an offline-mode server the derivation is **case-sensitive**: `Steve` and `steve` are
two different UUIDs, so they are two different accounts. That is vanilla server
behaviour, not something a plugin chooses.

---

## Resolving a Name to an Account

This is the part that used to bite. When a command takes a player name, the server can
normally translate it through its own name cache, so `/ban steve` finds `Steve`. But the
server only consults that cache **in online mode**. In offline mode it skips the cache
and derives a UUID from exactly what was typed, so `/ban steve` would have pointed at an
account nobody owns, and the command would have answered "unknown player" or, worse,
punished a ghost.

uxmEssentials therefore keeps **its own name index**: the name each account last joined
under, stored in the database and held in memory. A name is resolved in this order:

1. A player who is online right now (case-insensitive).
2. The plugin's name index (case-insensitive).
3. The server's own lookup, as a last resort.

The result: `/ban griefer`, `/eco give Griefer 100` and `/home visit GRIEFER` all reach
the same account, on online-mode and offline-mode servers alike, and commands that work
on absent players keep working when the player is offline.

### Filling the Index

- **On join.** Every login records the account and the exact name it joined under.
- **At first start.** The first time this version starts, the plugin seeds the index
  from the server's existing player files, so names from before the upgrade are known
  without waiting for those players to come back.
- **Size.** `lookup.name-index-size` in `config.conf` caps how many names are held in
  memory (default `50000`, roughly 6 MB). Names beyond that window still resolve when
  the case matches exactly. See
  [config.conf (Globals)](../config/global-config.md).

Nothing about this is offline-mode-only: online-mode servers use the same index and get
the same case-insensitive resolution without a Mojang round-trip.

---

## A Player Who Has Never Joined

A name the plugin has never seen has no account to point at. Commands that must act on
someone anyway (a vote arriving for a player who has not yet connected, for example)
fall back to the UUID the server would derive, so the reward or record is waiting for
them when they arrive. Commands that need an existing account (punishments, payments)
report that the player is unknown rather than inventing one.

---

## Skins

A cracked account has no Mojang skin of its own, but a name still has one wherever that
name exists as a real account, and that is what a `player:<name>` tablist entry or an
`/npc skin name:<name>` asks for.

uxmEssentials fetches those textures from Mojang directly. The platform's own profile
completion is not used, because it consults the session service only in online mode and
returns a profile with no textures at all on a cracked server. The lookup is shared and
cached server-wide, and `skins { mojang-lookup }` in `config.conf` turns it off entirely
for a server with no outbound network.

A player's *own* skin on a cracked server is a different matter: it comes from whatever
skin plugin you run, and uxmEssentials reads the live profile first, so whatever that
plugin put there is what renders.

---

## Networks

If your backends sit behind Velocity or BungeeCord, put the proxy in charge of identity
and switch the backends to `online-mode=false` with forwarding configured. The UUIDs are
then the proxy's, identical on every backend, and uxmEssentials's cross-server sync
(homes, economy, vaults, punishments) lines up as expected. See
[Velocity & Redis](../cross-server/overview.md).

---

## Next Steps

- [Core Concepts](concepts.md) for modules, config layout and reloading
- [Moderation](../features/moderation.md) for how punishment targets are named
- [Velocity & Redis](../cross-server/overview.md) for network setups
