---
title: Holograms
order: 42
description: The text above a spawner and what it can show.
icon: text-cursor
---

A hologram above each spawner shows its state. The module hooks whichever supported hologram
plugin is installed, automatically:

```yaml
hologram:
  enabled: true
  hologram-height: 1.5
```

| Supported | |
|---|---|
| DecentHolograms | |
| FancyHolograms | v2 and v3 |
| CMI | |

`hologram-height` is the vertical offset above the spawner block.

<Callout type="warning" title="No hologram plugin means turn this off">

The module has no renderer of its own. With none of the three installed, leave `enabled: false`.
Otherwise the plugin spends startup looking for a hook that will not be there.

</Callout>
