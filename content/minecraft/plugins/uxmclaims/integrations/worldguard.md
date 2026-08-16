---
title: WorldGuard
order: 360
icon: shield
---

## What It Does

When enabled:

- Players cannot create claims inside WorldGuard regions
- Prevents conflicts between claim protection and region protection

---

## Enabling

In `config.yml`:

```yaml
generalSettings:
  worldGuardSupport: true
```

---

## How It Works

1. Player tries to claim a chunk
2. uxmClaims checks if the chunk overlaps a WorldGuard region
3. If yes: Claim is blocked
4. If no: Claim proceeds normally

---

## Use Cases

### Protecting Spawn

If your spawn area is a WorldGuard region, players won't be able to claim chunks inside it.

### Server Events/Arenas

Mark event areas as WorldGuard regions to prevent claiming.

### Admin Builds

Protect important server builds from being claimed.

---

## Setup

1. Install WorldGuard (and WorldEdit)
2. Create regions for protected areas
3. Set `worldGuardSupport: true` in config.yml
4. Restart or reload the server

---

## Troubleshooting

### Players can still claim in regions

- Make sure the region exists: `/region info <name>`
- Make sure uxmClaims is detecting WorldGuard (check startup logs)
- Try restarting the server

### I want to allow claiming in some regions

Currently, all WorldGuard regions block claiming. Consider using `disabledWorlds` in config.yml for world-based
restrictions instead.
