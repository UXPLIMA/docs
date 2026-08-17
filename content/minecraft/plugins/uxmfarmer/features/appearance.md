---
title: Appearance
order: 39
icon: palette
---

What the farmer looks like is configured in `npc.yml` and `hologram.yml`, and most of it can be
handed to players as something they change themselves.

## The NPC

```yaml
name: "<#0388fc><bold>Farmer"
use-name: false
default-type: VILLAGER
default-baby: false
default-villager-profession: FARMER
default-glowing: true
default-glowing-color: "BLUE"
scale: 1.0
entity-specific-scales:
  VILLAGER: 2.5
```

`use-name: false` is the default because the hologram already carries the name — turning both on
gives you the name twice, once above the other.

`default-type` is any entity type, and works with Citizens and FancyNpcs. `entity-specific-scales`
lets a villager stand at 2.5 while everything else stays at 1.0, which is how the farmer reads as a
character rather than a mob.

Players change the entity type, the villager profession, the baby variant and the glow through the
appearance menus, gated by the `CHANGE_APPEARANCE`, `TOGGLE_GLOWING` and `CHANGE_GLOWING_COLOR`
rank permissions and by the `uxmfarmer.useglow` and `uxmfarmer.usebabyvariant` owner permissions.

Cooldowns on those changes are in `config.yml`:

```yaml
cooldowns:
  change-glowing-cooldown: 120
  change-entity-cooldown: 120
  change-profession-cooldown: 120
  move-cooldown: 180
```

## Skins

`skin-texture` and `skin-signature` are the default skin, as a texture value and its Mojang
signature. `skin-name` set to a player's name uses that player's skin instead.

Players with `uxmfarmer.changeskin` use `/farmer skin <name>`. You can also lock specific skins
behind permissions:

```yaml
permission-skins:
  "Notch": "uxmfarmer.usenotch"
```

Anything listed here is refused unless the player holds the matching permission, which makes
particular skins a rank reward.

## Models

If you run ModelEngine, BetterModel or ItemsAdder, the farmer can be a custom model instead of a
vanilla entity:

```yaml
model-settings:
  default-entity: VILLAGER
  model-source: ModelEngine
  model-name: "test"
```

Set `hooks.interaction-hook` to `ModelSystem` to use it. `default-entity` remains the entity the
model is attached to.

## The hologram

```yaml
enabled: true
see-through-blocks: false
shadow: true
text-opacity: 255
background-color: 1682722635
brightness: 1000
view-range: 30
height: 2.2
billboard: CENTER
text-alignment: CENTER
content:
  - "<#ff7700><bold>Farmer<#ff7700>"
  - ""
  - "<#ffcb0b>▶ Click to use the farmer"
```

Holograms are drawn by the plugin itself as text displays — there is no hologram plugin to install.
`content` is MiniMessage and supports placeholders, so the hologram can show the owner's name or
the storage state.

| Key | What it does |
|---|---|
| `view-range` | Blocks at which the hologram becomes visible |
| `height` | How far above the entity the text sits |
| `billboard` | `CENTER`, `FIXED`, `VERTICAL`, `HORIZONTAL` — which axes turn toward the viewer |
| `text-alignment` | `CENTER`, `LEFT`, `RIGHT` |
| `background-color` | ARGB integer; `0` for no background |
| `shadow` | Drop shadow behind the text |
| `see-through-blocks` | Whether the text renders through walls |

`entity-specific-height` sets the offset per entity type, because a bee and an iron golem do not
need their name in the same place. The default file covers the common types; add yours if you
enable an entity that is not listed.

<Callout type="tip" title="Working out background-color">

`background-color` is a single signed ARGB integer, not a hex string. The default `1682722635` is a
translucent dark panel. If you need a different one, compute the ARGB integer rather than guessing —
a wrong sign here produces an opaque block behind the text.

</Callout>

## Hiding the farmer

`farmer-visible-by-default: true` in `general-settings.yml` decides whether a new farmer has a body.
Players toggle it with `/farmer disappear` and the `NO_VISUAL` setting. A hidden farmer still works
— it harvests, sells and stores exactly as before, and is reachable through `/farmer menu`.

## Default click action

```yaml
default-click-actions:
  - "[menu] main"
  - "[sound] ENTITY_ALLAY_AMBIENT_WITHOUT_ITEM"
```

What happens when a player clicks the farmer. `[menu]` opens a menu by name and `[sound]` plays a
sound; changing the order changes which happens first.
