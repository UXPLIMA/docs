---
title: entitlements.yml
order: 260
icon: file-cog
---

## How This System Works

Every limit and cost is controlled by:

1. **Default Value** - What everyone gets without special permissions
2. **Permissions** - Players can get more/less based on permissions
3. **Strategy** - How multiple permissions combine
4. **Increment Rate** - How permissions calculate the value

---

## Understanding the Strategies

### STACK (Adding Up)

Adds all matching permissions together.

**Example:** Player has two permissions that give +5 claims each

- Result: 5 + 5 = **10 extra claims**

**Best for:** Claim limits where you want cumulative bonuses

---

### MAX (Highest Wins)

Takes the highest value and ignores the rest.

**Example:** Player has permissions for 100 chunks and 200 chunks

- Result: **200 chunks**

**Best for:** Hard limits like chunk count

---

### MIN (Lowest Wins)

Takes the lowest value.

**Example:** Player has permissions for $50 cost and $25 cost

- Result: **$25** (the discount applies)

**Best for:** Costs and delays (so VIPs pay less)

---

## Permission Format Tips

### Using Decimals in Permissions

Since permission nodes cannot contain dots for decimal values, use **underscores** instead:

| Permission | Parsed Value |
|------------|-------------|
| `uxmclaims.cost.time.0_01` | $0.01 per second |
| `uxmclaims.cost.chunk.5.25_99` | $25.99 for 5th chunk |
| `uxmclaims.cost.warptp.public.0_5` | $0.50 for public warp |

### Wildcard Permissions

You can use `*` to set a default price for all targets:

```
uxmclaims.cost.chunk.*.200    # All chunks cost $200
uxmclaims.cost.chunk.5.250    # 5th chunk costs $250 (overrides *)
```

With MIN strategy, the player gets the **lowest** matching price.

---

## All Entitlements Explained

### 🏠 Claim Limit

```yaml
uxmclaims.limit.claim:
  strategy: STACK
  defaultValue: 1
  incrementRate: 1
```

**What it does:** How many separate claims a player can own

| Setting         | Value | Meaning                             |
|-----------------|-------|-------------------------------------|
| `strategy`      | STACK | Permissions stack together          |
| `defaultValue`  | 1     | Everyone starts with 1 claim        |
| `incrementRate` | 1     | Each permission level gives 1 more  |

**How permissions work:**

- `uxmclaims.limit.claim.2` → 2 extra claims (total: 3)
- `uxmclaims.limit.claim.5` → 5 extra claims (total: 6)

**Total formula:** defaultValue + (sum of permission levels)

---

### 🧱 Chunk Limit

```yaml
uxmclaims.limit.chunk:
  strategy: STACK
  defaultValue: 10
  incrementRate: 1
```

**What it does:** Total chunks a player can claim (across all claims)

| Setting         | Value | Meaning                        |
|-----------------|-------|--------------------------------|
| `strategy`      | STACK | Permissions stack together     |
| `defaultValue`  | 10    | Everyone gets 10 chunks        |
| `incrementRate` | 1     | Permission equals exact number |

**How permissions work:**

- `uxmclaims.limit.chunk.200` → Exactly 200 chunks
- `uxmclaims.limit.chunk.500` → Exactly 500 chunks

---

### 👥 Member Limit

```yaml
uxmclaims.limit.member:
  strategy: STACK
  defaultValue: 50
  incrementRate: 1
```

**What it does:** Maximum members per claim

**How it works:** Same as chunk limit - the permission number is the limit.

---

### ⏰ Claim Time Limit

```yaml
uxmclaims.limit.time:
  strategy: STACK
  defaultValue: 2592000000
  incrementRate: 1
```

**What it does:** Maximum claim duration in milliseconds

| Duration | Milliseconds  |
|----------|---------------|
| 1 day    | 86,400,000    |
| 7 days   | 604,800,000   |
| 30 days  | 2,592,000,000 |

---

### 💵 Creation Cost

```yaml
uxmclaims.cost.claim:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0
```

**What it does:** Money required to create a new claim

| Setting         | Value | Meaning                                  |
|-----------------|-------|------------------------------------------|
| `strategy`      | MIN   | Lowest cost permission wins (discounts!) |
| `defaultValue`  | 0.0   | First claim is free!                     |
| `incrementRate` | 0.0   | Flat pricing, no escalation              |

---

### 🧱 Chunk Purchase Cost

```yaml
uxmclaims.cost.chunk:
  strategy: MIN
  defaultValue: 25.0
  incrementRate: 0.0
```

**What it does:** Money to expand claims (buy chunks)

`incrementRate: 0.0` means the price is **flat** - every chunk costs $25.

---

### ⏱️ Teleport Delay

```yaml
uxmclaims.delay.teleport:
  strategy: MIN
  defaultValue: 3
  incrementRate: 1
```

**What it does:** Seconds to wait before teleporting

**VIP example:**

- `uxmclaims.delay.teleport.0` → Instant teleport (0 seconds)
- `uxmclaims.delay.teleport.1` → 1 second delay

---

### 📍 Warp Limit

```yaml
uxmclaims.limit.warp:
  strategy: MAX
  defaultValue: 3
  incrementRate: 1
```

**What it does:** Maximum warps per claim (default: 3)

---

### 🎭 Role Limit

```yaml
uxmclaims.limit.role:
  strategy: MAX
  defaultValue: 5
  incrementRate: 1
```

**What it does:** Maximum custom roles per claim (excludes Owner, Member, Default)

---

### 🚫 Ban Limit

```yaml
uxmclaims.limit.ban:
  strategy: MAX
  defaultValue: 25
  incrementRate: 1
```

**What it does:** Maximum players you can ban per claim (default: 25)

---

### 📧 Invite Limit

```yaml
uxmclaims.limit.invite:
  strategy: MAX
  defaultValue: 10
  incrementRate: 1
```

**What it does:** Maximum pending invites per claim

---

### 💰 Various Costs

```yaml
uxmclaims.cost.warp:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0

uxmclaims.cost.role:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0

uxmclaims.cost.warptp:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0

uxmclaims.cost.invite:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0

uxmclaims.cost.rename:
  strategy: MIN
  defaultValue: 0.0
  incrementRate: 0.0

uxmclaims.cost.time:
  strategy: MIN
  defaultValue: 5.0
  incrementRate: 0.0
```

| Entitlement   | What It Costs            |
|---------------|--------------------------|
| `cost.warp`   | Creating a warp          |
| `cost.role`   | Creating a custom role   |
| `cost.warptp` | Teleporting to a warp    |
| `cost.invite` | Sending an invite        |
| `cost.rename` | Renaming a claim         |
| `cost.time`   | Extending claim duration |

---

## Example: Setting Up Ranks

### Free Player (Default)

No special permissions needed.

- 2 claims
- 100 chunks
- $100 per claim

### VIP Rank

Permissions to give:

```
uxmclaims.limit.claim.3    → 2 + (3×20) = 62 claims
uxmclaims.limit.chunk.300  → 300 chunk limit
uxmclaims.cost.claim.50    → Claims cost $50
uxmclaims.delay.teleport.0 → Instant teleport
```

### MVP Rank

```
uxmclaims.limit.claim.5    → 2 + (5×20) = 102 claims
uxmclaims.limit.chunk.500  → 500 chunk limit
uxmclaims.cost.claim.25    → Claims cost $25
uxmclaims.delay.teleport.0 → Instant teleport
uxmclaims.limit.warp.20    → 20 warps per claim
```

---

## Tips

1. **Use STACK for bonuses** - Claim limits that should add up
2. **Use MAX for hard limits** - Chunk/member counts
3. **Use MIN for costs** - So VIPs pay less
4. **Set incrementRate: 0** for flat pricing
5. **Test with actual players** - Make sure the math works!

---

## Next Steps

- [📝 messages.yml](../config/messages-yml.md) - Customize all plugin messages
- [🔔 webhooks.yml](../config/webhooks-yml.md) - Set up Discord notifications
