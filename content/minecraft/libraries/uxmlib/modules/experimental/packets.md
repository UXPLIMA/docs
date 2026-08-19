---
title: Packets
order: 312
description: Mojang-mapped packet ports for NPCs, the tab list and text displays, with the NMS quarantined behind them.
icon: boxes
---

`uxmlib-packet` holds the shared helpers and three ports. Every packet crosses a port boundary as an
opaque `Object`, so the interface and everything above it carries no `net.minecraft` reference and is
testable against a fake. Only the single `internal` implementation behind each port touches NMS.

## NPC packets

```java
NpcPackets npcs = new NmsNpcPackets(sender);

int id = npcs.allocateEntityId();
```

A fake-player NPC reaches a client in a fixed sequence: a player-info entry carrying the name and
skin so the client knows how to render a body, then the entity spawn, then the look packets that aim
the head and body.

The client links the skin to the spawned entity because the spawn UUID equals the player-info entry's
profile id, so the spawn must reuse the same profile id.

The entry is added **unlisted** so the body renders but the NPC never appears as a tab-list row, and
the entry is kept for the entity's whole lifetime. Removing it de-renders the entity on modern
clients, which is the single most common reason a hand-rolled NPC vanishes after a few seconds.

Supporting value types: `NpcPose`, `EquipmentSlot`, `ArmorStandPart`, `HorseVariant`, `NamedColor`,
`ByteAngle`.

## Tab list packets

```java
TabListPackets tabs = new NmsTabListPackets(...);

tabs.addOrUpdate(entry);
```

These are how you do the per-viewer tab things Paper cannot: a custom display name, a client-side
sort order, and a custom skin for an entry you fully control.

`TabEntry` describes the row and `TabSkin` its texture. `PlayerInfoUpdates` builds the update actions.

Each method returns one built packet, so the same packet can be sent to many viewers without
rebuilding it. That matters: building is the expensive half.

## Text display packets

`DisplayTextPackets` is the per-viewer text-display port, which is what a per-viewer hologram sits on
when the public `Display` API cannot express it.

## Shared helpers

| Helper | Does |
|---|---|
| `Components` | Adventure to vanilla component conversion |
| `Bundles` | Grouping packets so a client applies them in one frame |
| `Codecs` | The stream-codec buffer trick for building payloads |
| `Reflect` | Guarded reflection, so a missing member is a clear failure |
| `EntityIds` | Fake-entity id allocation off the shared server counter |
| `GameProfiles` | Profile and texture property construction |

Bundling matters more than it looks: an NPC spawn that arrives as four separate packets can render
for a frame with the wrong pose or skin. Bundled, the client applies them together.

## Sending

```java
PacketSender sender = new PacketSender(resolver);

sender.send(player, packet);
sender.send(channel, packet);
```
