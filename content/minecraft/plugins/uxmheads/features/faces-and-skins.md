---
title: Faces and skins
order: 301
description: How a skin is found, fetched, kept and turned into a face.
icon: scan-face
---

## Finding the skin

A player's profile names the address of their skin. The plugin downloads it only from a host listed under
`skins.hosts` (`textures.minecraft.net` out of the box), because a player's own client can put any address there. An
address on another host, or one that is not `http` or `https`, is never fetched.

- A player whose profile names no skin wears the skin their name has on the account service while
  `skins.find-by-name` is on. That covers servers that let players in without an account. It is also what
  `%uxmheads_face_<row>_<name>%` reads for a player who is not on the server.
- A skin that cannot be read, and a player with no skin when `find-by-name` is off, wear `face.fallback`.

## Fetching

One download may take `skins.timeout` (5s). A login waits up to `skins.wait-at-login` (2s) for a skin this server has
never seen, so the first welcome already shows it; a skin seen before is read from disk at once. `0s` never waits, and
the first welcome of a new skin then shows the fallback.

## Keeping

- Downloaded skins are kept in `skins/` and read from disk next time. A kept file that cannot be read is downloaded
  again. A file nobody wears for `skins.keep-days` is deleted.
- `skins.memory` faces are held in memory. A face is 64 colours, so a thousand is a small fraction of a megabyte.
- A placeholder answers from memory: a face that has not loaded yet answers the fallback, and the next ask answers the
  real one.

## Drawing

The face is the 8 by 8 face square of the skin, with the hat layer drawn over it while `face.hat` is on. An old 64 by
32 skin whose hat area is fully opaque counts as wearing no hat, because those skins filled it with a solid colour. A
high resolution skin is averaged down to 8 by 8. `face.hat = false` draws the bare face, hiding a hat, a helmet or
glasses painted on that layer.

`/uxmheads refresh <player>` reads a player's skin again after they changed it.

Related: [config.conf](../config/config-conf.md#skins)
