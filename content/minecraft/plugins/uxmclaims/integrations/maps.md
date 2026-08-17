---
title: Map plugins
order: 804
description: Drawing claims on Dynmap, BlueMap, Pl3xMap and squaremap.
icon: map
---

Four are supported and detected automatically. Install one; the plugin picks it up.

| Plugin |
|---|
| Dynmap |
| BlueMap |
| Pl3xMap |
| squaremap |

```yaml
visualizer:
  enabled: true
```

## Styling

```yaml
visualizer:
  style:
    borderColor: "#33FF33"
    borderOpacity: 0.8
    borderWeight: 2
    fillColor: "#33FF33"
    fillOpacity: 0.35
```

| Key | Default | Meaning |
|---|---|---|
| `borderColor` | `#33FF33` | Outline colour |
| `borderOpacity` | `0.8` | Outline opacity, 0–1 |
| `borderWeight` | `2` | Outline thickness in pixels |
| `fillColor` | `#33FF33` | Fill colour |
| `fillOpacity` | `0.35` | Fill opacity, 0–1 |

These are the web map's own colour format — six hex digits, no alpha channel. Opacity is the separate
`…Opacity` key. That is different from the in-world `border` colours in `config.yml`, which are
eight-digit `#RRGGBBAA`.

## Tooltips

The hover popup is HTML, rendered by the map's front end, with placeholders parsed first.

```yaml
visualizer:
  tooltip:
    lines:
      - '<span style="font-weight:bold; font-size:12px; color:#22c55e;">%claim.name%</span>'
      - '<span style="color:#22c55e;">• Owner:</span> <span style="color:#111827;">%claim.owner.name%</span>'
      - '<span style="color:#22c55e;">• Chunks:</span> <span style="color:#111827;">%count:claim.chunks%</span>'
      - '<span style="color:#22c55e;">• Members:</span> <span style="color:#111827;">%count:claim.members%</span>'
      - '<span style="color:#22c55e;">• Created:</span> <span style="color:#111827;">%date:claim.creationDate%</span>'
```

MiniMessage does **not** work here — `<gray>` renders as a broken HTML tag. Use inline CSS, as the
shipped lines do.

Any [placeholder](../placeholders/reference.md) works, including the `count:`, `date:` and `time:`
modifiers.

<Callout type="warning" title="The tooltip is public">

Whatever you put in these lines is visible to anyone who can load the web map, signed in or not.
`%claim.owner.name%` is fine; coordinates of a claim's vault or spawn are an invitation. Keep the
tooltip to the name, the owner and counts.

</Callout>

## Notes

- **Only one map plugin is used.** With several installed the first detected wins.
- **The whole claim is drawn, not just the main chunk** — every chunk it owns, as one outline.
- **`enabled: false` removes the markers entirely,** which is what you want on a server where the map
  is meant to hide player builds.
