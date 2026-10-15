---
title: Map plugins
order: 804
description: Drawing claims on Dynmap, BlueMap, Pl3xMap and squaremap.
icon: map
---

uxmClaims draws every claimed chunk on the web map of Dynmap, BlueMap, Pl3xMap or squaremap. Install
one of them; the plugin finds it at startup.

## Settings

```yaml
visualizer:
  enabled: true
  style:
    borderColor: "#33FF33"
    borderOpacity: 0.8
    borderWeight: 2
    fillColor: "#33FF33"
    fillOpacity: 0.35
```

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `true` | Draw claims on the web map. Read at startup |
| `style.borderColor` | `#33FF33` | Outline colour |
| `style.borderOpacity` | `0.8` | Outline opacity, 0 to 1 |
| `style.borderWeight` | `2` | Outline thickness |
| `style.fillColor` | `#33FF33` | Fill colour |
| `style.fillOpacity` | `0.35` | Fill opacity, 0 to 1 |
| `tooltip.lines` | Name, owner, chunks, members, created | The hover text, joined with line breaks |

Colours here are six hex digits with opacity in its own key. The in-world `border` colours in
`config.yml` are eight-digit `#RRGGBBAA` instead.

## Which map is used

Only one map plugin is used, the first found in this order:

| Order | Plugin | Layer |
|---|---|---|
| 1 | Dynmap | Marker set `Claims` |
| 2 | Pl3xMap | Layer `Claims` in each world |
| 3 | BlueMap | Marker set `Claims` on every map of the world, drawn from bottom to top of the world |
| 4 | squaremap | Layer `Claims` in each world |

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

MiniMessage does not work here: `<gray>` arrives as an unknown HTML tag. Use inline CSS, as the
shipped lines do. Any [claim placeholder](../placeholders/reference.md) works, including the
`count:`, `date:` and `time:` modifiers.

<Callout type="warning" title="The tooltip is public">

Anyone who can load the web map sees these lines. The name, the owner and counts are fine;
coordinates of a claim's spawn or warps tell everyone where to go. Keep the tooltip to what you would
post publicly.

</Callout>

## Notes

- **Each chunk is its own square**, labelled with the claim name, so a claim shows as a group of
  adjacent squares rather than one outline.
- **Markers are redrawn** when a claim is created, deleted or expires and when a chunk is added or
  removed. A rename or a new member shows on the map after the next of those changes or a restart.
- **All markers are rebuilt** one second after startup.
- **`enabled: false`** draws nothing, which suits a server where the map should not reveal player
  builds.

Related: [Overview](overview.md), [config.yml](../config/config-yml.md)
