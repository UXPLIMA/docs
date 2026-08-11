---
title: Regions (WorldGuard)
order: 1070
---

## Regions Commands

The `regions` module manages **WorldGuard** regions in-game: list a world's regions, define
a new cuboid, cycle its protection flags, edit its member and owner rosters, and set its
priority: all from `/regions`, all through **Brigadier** with tab-completed world names,
region ids, and player names.

<Callout type="info" title="WorldGuard is a soft-dependency">

Every command below needs **WorldGuard** installed. With WorldGuard absent, the module
loads but is **inert**: each command answers with a single *"WorldGuard not installed"*
line and nothing opens. `/regions create` additionally uses **WorldEdit** to read your
selection; without WorldEdit, mark the corners with `/regions pos1` and `/regions pos2`.
See the [Regions feature guide](../features/regions.md) for the details.

</Callout>

---

## Command Reference

| Command | Description | Permission |
|---------|-------------|------------|
| `/regions [world]` | Paginated list of a world's regions (id, priority, member count); click one to open its detail panel | `uxmessentials.regions.list` |
| `/regions create <id>` | Create a cuboid region from your WorldEdit selection, or from the marked corners | `uxmessentials.regions.create` |
| `/regions pos1` | Mark the first corner at your current block | `uxmessentials.regions.create` |
| `/regions pos2` | Mark the second corner at your current block | `uxmessentials.regions.create` |
| `/regions flags <id>` | Open the flag editor: click a flag to cycle `UNSET → ALLOW → DENY` | `uxmessentials.regions.flags` |
| `/regions members <id>` | Open the owner/member roster: click an entry to remove it | `uxmessentials.regions.members` |
| `/regions addmember <id> <player>` | Add a member (offline-safe name → UUID) | `uxmessentials.regions.members` |
| `/regions addowner <id> <player>` | Add an owner (offline-safe name → UUID) | `uxmessentials.regions.members` |
| `/regions priority <id> <value>` | Set the region's priority | `uxmessentials.regions.admin` |

The flag editor and the member/owner roster also open straight from a region's **detail
panel**, so once you've clicked into a region from `/regions`, you rarely type the `<id>`
forms by hand.

---

## Defining a Region

A region is made in two steps: pick the volume, then name it.

### With WorldEdit

```
//wand
// select the two corners with the wand
/regions create arena
```

`/regions create` reads your WorldEdit selection directly.

### Without WorldEdit

```
/regions pos1        # stand at one corner
/regions pos2        # stand at the opposite corner
/regions create arena
```

The marked corners are remembered until you run `create`. Creating a region whose `<id>`
already exists, or creating with no selection and no marked corners, is rejected with a
clear message.

---

## Editing Flags & Rosters

```
/regions flags arena         # cycle pvp, build, entry, … one click at a time
/regions members arena       # owners first, click to remove
/regions addowner arena Notch
/regions addmember arena Steve
/regions priority arena 10
```

<Callout type="note" title="State-flags only">

The flag editor cycles WorldGuard **state flags** (allow/deny toggles). Free-text flags
such as `greeting` are out of scope: edit those with WorldGuard's own commands. Which
flags appear is set by `flags { editable = [ … ] }` in `modules/regions/config.conf`.

</Callout>

<Callout type="note" title="Rename or disable any of these">

Every command literal above can be renamed, re-aliased, or switched off in
`commands/commands.conf` without touching permissions, see
[Renaming Commands](../config/commands-conf.md).

</Callout>

---

## Next Steps

- [🛡️ Regions Feature Guide](../features/regions.md): the WorldGuard soft-dependency, the flag editor, and rosters explained
- [🔐 Permission Reference](../permissions/reference.md): every `uxmessentials.regions.*` node
- [🧩 Per-Module Config](../config/per-module.md): `modules/regions/config.conf`
