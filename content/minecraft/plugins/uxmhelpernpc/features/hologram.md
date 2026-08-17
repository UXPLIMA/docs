---
title: Hologram
order: 34
icon: text-cursor
---

The floating text above an NPC is drawn by the plugin itself, not by your hologram plugin, so it
works the same under every interaction hook.

```yaml
hologram:
  enabled: true
  billboard: CENTER        # CENTER, FIXED, VERTICAL, HORIZONTAL
  view-range: 30
  height: 2.2
  scale: 1, 1, 1
  update-period: 1200
  see-through-blocks: false
  shadow: true
  text-opacity: 255
  background-color: 1682722635
  brightness: 1000
  text-alignment: CENTER   # CENTER, LEFT, RIGHT
  content:
    - "<rainbow>Helper NPC"
    - "<#ffcb0b>▶ Click to open info menu<reset>"
```

`content` is MiniMessage and accepts PlaceholderAPI placeholders, so the hologram can show island
level, owner name or anything else your other plugins expose.

<Callout type="tip" title="update-period is a performance setting">

`update-period` is in ticks — 1200 is one minute. Every tick of that period is a text refresh for
every NPC on the server. Keep it high unless the hologram shows something that genuinely changes
by the second. Changing it needs a restart, not a reload.

</Callout>

## Height

`height` lifts the text above the entity, and `entity-specific-height` overrides it per entity
type so the text sits just above a cat and just above an iron golem alike:

```yaml
entity-specific-height:
  VILLAGER: 2.1
  IRON_GOLEM: 2.9
  CAT: 0.7
  CAMEL: 3.7
```

## background-color

`background-color` is a packed ARGB integer, the same value a text display entity takes. Set the
alpha to zero for a floating text with no panel behind it.

## The NPC's own name

`npc.use-name` is off by default, because the hologram already carries the name. Turn it on if you
want the entity nameplate as well:

```yaml
npc:
  name: "<rainbow><bold>Helper NPC"
  use-name: false
```
