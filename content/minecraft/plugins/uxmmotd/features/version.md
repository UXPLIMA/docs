---
title: The version text
order: 305
description: Replacing the protocol name with your own text, and why it ships off.
icon: tag
---

| Key | Default | Meaning |
|---|---|---|
| `version.enabled` | `false` | Replace the protocol name with `version.text` |
| `version.text` | `<good>● <b>1.21 - 26.2</b>` | The replacement |
| `version.force-mismatch` | `false` | Send protocol -1 so the text always shows |

<Callout type="warning" title="Every client marks the entry incompatible">

A client shows the protocol name only when its protocol does not match the server's, so the text needs
`force-mismatch`. The server then sends protocol -1, no client reports that number, and every client of every version
draws "Incompatible version!" with a red cross where the ping bars belong. They all still join. The line reads well and
the entry beside it reads as broken, which is why this ships off.

</Callout>

Maintenance has its own `maintenance.version` with the same three keys, also off: a closed server that marks itself as
the wrong version tells the player the one thing that is not true.

Related: [config.conf](../config/config-conf.md#version)
