---
title: Installing a system
order: 2
description: Four steps that are the same for every UXR system.
icon: download
---

Every system ships as one folder. Installing it is the same four steps whichever
product you bought.

## 1. Put the folder in ServerScriptService

Open your place in Roblox Studio and drag the package folder into
**ServerScriptService**. The folder is named after the product, for example
`uxrDailyRewardsSystem`.

That is the whole installation. Nothing goes in `Workspace`, nothing goes in
`StarterGui`, nothing goes in `ReplicatedStorage`. Those are created for you.

## 2. Press Play once

On the first server start the package installs itself: it copies its shared code into
`ReplicatedStorage`, its assets into `ServerStorage`, its interface into `StarterGui`,
and creates the remotes. See [The bootstrap](bootstrap.md) for what happens in what
order.

If the Output window shows the system's log prefix and no errors, it is installed.

## 3. Edit the settings

Open `Shared/Config/Settings.luau` inside the package. Every field is commented in
plain English, and the values on the right of the `=` are the ones to change. Do not
rename the keys on the left.

Each product's own Configuration page lists its fields with their defaults.

## 4. Publish

Settings are read from the file, so publishing the place ships them. There is no
external dashboard and no license check at runtime.

## Requirements

| | |
|---|---|
| Roblox Studio | Any current version |
| API access | **Studio Access to API Services** must be on for saving to work |
| Third-party code | None. No external modules, no HTTP calls |

Saving uses `DataStoreService`. In Studio that needs **Game Settings, Security,
Enable Studio Access to API Services**. Without it the system still runs, but progress
resets every session, and you will see load warnings in Output.

## Installing more than one system

They coexist. Each package uses its own folder name, its own remotes folder, its own
log prefix and its own DataStore, so two systems never collide.

<Callout type="warning" title="Two systems selling Robux products need one owner of ProcessReceipt">

`MarketplaceService.ProcessReceipt` is a single global callback: the last script to set
it wins. Any UXR system that sells a Developer Product has a
`ManageProcessReceipt` setting. Leave it `true` on exactly one system, set it `false` on
the rest, and call each of their `handleReceipt(receiptInfo)` functions from your own
callback.

Get this wrong and purchases in the other systems are charged but never delivered.

</Callout>

## Updating

Replace the package folder with the new one, then re-apply your settings. Keep a copy of
your `Settings.luau` and any file the product describes as buyer-owned, for example a
rewards ladder or a hooks file, before you overwrite.

Player data is not touched by an update: it lives in a DataStore keyed by the `Store`
name in your settings, not in the package.
