---
title: Map Plugins
order: 370
---

## Supported Map Plugins

| Plugin    | Support |
|-----------|---------|
| Dynmap    | ✅ Full  |
| BlueMap   | ✅ Full  |
| Pl3xMap   | ✅ Full  |
| SquareMap | ✅ Full  |

---

## Enabling

In `config.yml`:

```yaml
visualizer:
  enabled: true
```

The plugin automatically detects which map plugin you have installed.

---

## Customizing Appearance

### Border and Fill Style

```yaml
visualizer:
  style:
    borderColor: "#33FF33"     # Line color (hex)
    borderOpacity: 0.8         # Line opacity (0-1)
    borderWeight: 2            # Line thickness
    fillColor: "#33FF33"       # Fill color (hex)
    fillOpacity: 0.35          # Fill opacity (0-1)
```

**Example colors:**

- Green: `#33FF33`
- Red: `#FF3333`
- Blue: `#3333FF`
- Yellow: `#FFFF33`

---

### Tooltip (Hover Information)

```yaml
visualizer:
  tooltip:
    lines:
      - '<span style="font-weight:bold;">%claim.name%</span>'
      - '• Owner: %claim.owner.name%'
      - '• Chunks: %count:claim.chunks%'
      - '• Members: %count:claim.members%'
      - '• Created: %date:claim.creationDate%'
```

This is HTML that shows when hovering over a claim on the map.

**Available placeholders:**

- `%claim.name%` - Claim name
- `%claim.owner.name%` - Owner name
- `%count:claim.chunks%` - Number of chunks
- `%count:claim.members%` - Number of members
- `%date:claim.creationDate%` - Creation date

---

## How It Looks

On the web map, claims appear as colored shapes:

- The border shows the outline
- The fill shows the area
- Hovering shows the tooltip

---

## Setup

1. Install a map plugin (Dynmap, BlueMap, etc.)
2. Set `visualizer.enabled: true` in config.yml
3. Customize colors if desired
4. Restart or reload the server
5. View your web map!

---

## Troubleshooting

### Claims don't appear on map

- Make sure `enabled: true` is set
- Check that the map plugin is working
- Restart both plugins
- Check console for errors

### Colors are wrong

- Use hex format with `#` (e.g., `"#33FF33"`)
- Make sure values are in quotes

### Tooltip shows raw placeholders

- Check that placeholders are spelled correctly
- HTML formatting must be valid
