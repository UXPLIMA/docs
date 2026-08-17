---
title: Appearance
order: 32
description: Entity type, skins, glow, models and what players may change themselves.
icon: palette
---

## Entity type

`npc.default-type` is what a new NPC looks like. Any Bukkit entity type works, but only the
Citizens and FancyNpcs hooks change shape per NPC: the other hooks draw whatever they draw.

Players change their own NPC's type from the appearance menu, which fires the
`[modify_appearance]` action with an entity type as its argument.

## Skins

```yaml
npc:
  skin-texture: "ewogICJ0aW1lc3RhbXAi..."
  skin-signature: "JwgV4AiVpcl914qub..."
  skin-name: ""
  permission-skins:
    "Notch": "uxmhelpernpc.usenotch"
```

`skin-texture` and `skin-signature` are the server-wide default. `skin-name` fetches a skin by
player name instead. A player switches their own with `/helpernpc skin <name>`; a name listed in
`permission-skins` needs the node beside it. `/helpernpc skin reset` returns to the default, and a
name the skin service cannot resolve is rejected and reverted.

## Glow

```yaml
npc:
  default-glowing: true
  default-glowing-color: "BLUE"
```

The glow menu writes a colour through the `[glow_color]` action and toggles the outline with
`[glow_status]`. Picking the colour that is already active is refused with the
`already-using-that-glow-color` sound, and colour changes are rate-limited by
`cooldown.change-glowing-cooldown`.

## Scale

```yaml
npc:
  scale: 1.0
  entity-specific-scales:
    VILLAGER: 2.5
```

`scale` is the default; `entity-specific-scales` overrides it per entity type, because a villager
and a camel do not read the same size at the same scale.

## Models

With `interaction-hook: ModelSystem` the NPC is a model instead of a vanilla entity:

```yaml
model-settings:
  default-entity: VILLAGER
  model-source: ModelEngine   # ModelEngine, ItemsAdder or BetterModel
  model-name: "test"
```

`default-entity` is the entity the model rides, and `model-name` is the model id in whichever
system you named.

## Hiding without deleting

`/helpernpc disappear` sets the NPC to no-visual: it stays in the database, keeps its settings and
stops being drawn. Moving is refused while an NPC is hidden.
