---
title: Vault & Treasury
order: 1490
description: Serving our economy to other plugins, and running on somebody else's.
icon: landmark
---

uxmEssentials ships its own database-backed economy. Vault and Treasury do not power it; they are the
bridges that let other plugins (shops, jobs, crates) read and write those balances.

## Provider vs. Consumer: the important distinction

There are two completely separate things going on, and mixing them up is the usual
source of confusion:

| Role | What it means | Who talks to whom |
|------|---------------|-------------------|
| **Provider** (uxmEssentials serves) | uxmEssentials publishes its wallet as *the* server economy through Bukkit's `ServicesManager`. Other plugins call it to check and change balances. | Shop / jobs / crate plugins to uxmEssentials |
| **Consumer** (uxmEssentials reads) | Inside menus, uxmEssentials can read *other* currency systems (Vault money, XP, PlayerPoints, ...) for conditions and actions. | uxmEssentials to another economy plugin |

Neither side needs the other to be installed. The native ledger works on a bare
Paper server with no economy plugin at all.

## Serving Your Economy (the Provider side)

On enable, the economy module runs a **register-or-defer** handshake through the
Bukkit `ServicesManager`:

1. It looks for a foreign economy already registered: **Treasury first, then
   Vault**.
2. If one is found, uxmEssentials **defers** to it (it won't fight another plugin
   for the economy slot).
3. If none is found, it **registers its own** native DB-backed provider at
   `ServicePriority.Normal`.

Once registered, plugins that speak Vault or Treasury transparently read balances,
charge for purchases and pay out rewards against the uxmEssentials wallet: no extra
glue needed.

<Callout type="note" title="Vault is single-currency">

Vault has no concept of multiple currencies. Only your **default** currency is
exposed over the Vault bridge; a Vault request against any other currency is
rejected with `CURRENCY_UNSUPPORTED`. Use **Treasury** (or the in-game
`/exchange`) if you need multi-currency operations to reach third-party plugins.

</Callout>

You control the provider handshake in `modules/economy/config.conf`:

```
economy {
  provider {
    register = true      # register our economy through ServicesManager
    priority = "Normal"  # ServicePriority when we register
  }
}
```

Set `register = false` if you deliberately want another plugin to own the economy
slot and uxmEssentials to keep its wallet private.

## Reading Other Currencies (the Consumer side)

Separately, the [menu engine](../menus/actions-requirements.md) can target a
currency backend by name in menu actions (`give-money:`, `take-money:`,
`has-money:`, ...). This is how one GUI button can charge Vault money while another
spends XP:

| Spec | Reads from | Needs a plugin? |
|------|------------|-----------------|
| `vault` | The Vault economy provider | Vault + an economy |
| `exp` | Native Paper experience | No |
| `playerpoints` | PlayerPoints | Yes |
| `coinsengine[:name]` | CoinsEngine (optionally a named currency) | Yes |
| `zessentials[:name]` | zEssentials (optionally a named currency) | Yes |

An unknown or absent backend simply resolves to a provider that never reports
itself `available()`, logging a single warning: the menu still opens, that one
currency check just fails closed.

## When Vault and Treasury Are Absent

| Situation | Behavior |
|-----------|----------|
| Neither Vault nor Treasury installed | The native DB ledger is authoritative and fully multi-currency. Everything works; other plugins just can't see the balances. |
| Economy module disabled | Paid features across the plugin (home costs, warp fees, vault creation, ...) become **free**. |
| Vault present, non-default currency requested | Rejected with `CURRENCY_UNSUPPORTED` (see the note above). |

Vault and Treasury are pure soft-depends: their SDK classes are only touched behind
a plugin-present guard, so a missing bridge loads none of their code.

## Setup Checklist

1. Keep the **economy** module enabled (it is, by default).
2. To let shops and other plugins use your balances, install **Vault** (or
   **Treasury**): no further config needed; uxmEssentials registers automatically.
3. If another plugin should own the economy slot, install *its* provider first and
   set `provider.register = false`.
4. Restart and run `/uxmess doctor`: it reports which economy provider is active.

Related: [Economy Feature Guide](../modules/economy.md), [Menu Actions & Requirements](../menus/actions-requirements.md), [‍ Developer Overview](../developer/overview.md)
