---
title: PlaceholderAPI
order: 350
icon: braces
---

## Enabling

In `config.yml`:

```yaml
generalSettings:
  placeholderSupport: true
```

---

## Available Placeholders

Use these in any plugin that supports PlaceholderAPI:

### Location Check

| Placeholder | Output |
|-------------|--------|
| `%uxmclaims_is_claimable%` | `true` if current location can be claimed, `false` otherwise |

### Current Claim (where player is standing)

| Placeholder | Output |
|-------------|--------|
| `%uxmclaims_claim_name%` | Claim name |
| `%uxmclaims_claim_owner_name%` | Owner's username |
| `%uxmclaims_claim_owner_uid%` | Owner's UUID |

### Claim Statistics

| Placeholder | Output |
|-------------|--------|
| `%uxmclaims_count:claim.chunks%` | Number of chunks |
| `%uxmclaims_count:claim.members%` | Number of members |
| `%uxmclaims_count:claim.warps%` | Number of warps |
| `%uxmclaims_count:claim.roles%` | Number of roles |

### Claim Time

| Placeholder | Output |
|-------------|--------|
| `%uxmclaims_time:claim.remainTime%` | Formatted remaining time |
| `%uxmclaims_date:claim.expireDate%` | Expiration date |
| `%uxmclaims_date:claim.creationDate%` | Creation date |

---

## Default Values

When a player isn't in a claim, you can customize what shows in `messages.yml`:

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'Wilderness'
  uxmclaims_claim_owner_name: 'Nobody'
```

---

## Example Uses

### In a Scoreboard

```
You are in: %uxmclaims_claim_name%
Owner: %uxmclaims_claim_owner_name%
```

### In Chat Format

```
[%uxmclaims_claim_name%] PlayerName: message
```

### In Tab/Nameplates

```
PlayerName
%uxmclaims_claim_name%
```

---

## Setup

1. Install PlaceholderAPI
2. Set `placeholderSupport: true` in config.yml
3. Restart or reload the server
4. Use the placeholders in your other plugins

---

## Troubleshooting

### Placeholders show as raw text

- Make sure PlaceholderAPI is installed
- Make sure the other plugin supports PlaceholderAPI
- Try `/papi parse me %uxmclaims_claim_name%` to test
