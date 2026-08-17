---
title: Quick Installation
order: 30
description: Requirements, supported server software, and the first start.
icon: download
---

## Requirements

Before you start, make sure you have:

| Requirement          | Minimum Version                 |
|----------------------|---------------------------------|
| **Minecraft Server** | Spigot, Paper, Purpur, or Folia |
| **Server Version**   | 1.19.4 or newer                 |
| **Java**             | Java 21 or newer                |

<Callout type="note" title="Compatibility">

uxmClaims works on **Spigot**, **Paper**, **Purpur**, **Folia** and 
other Bukkit/Spigot forks. It does **not** require Paper, but we 
recommend it for better performance.

</Callout>

---

## Step-by-Step Installation

### Step 1: Download the Plugin

Download the latest `uxmClaims.jar` file from the official source.

### Step 2: Put It in Your Plugins Folder

1. Go to your server folder
2. Open the `plugins` folder
3. Drop the `uxmClaims.jar` file inside

Your folder should look like this:

```
your-server/
├── plugins/
│   ├── uxmClaims.jar  ← Put it here!
│   └── (other plugins)
├── server.properties
└── paper.jar
```

### Step 3: Start Your Server

Start (or restart) your server. You'll see messages like:

```
[uxmClaims] Loading uxmClaims...
[uxmClaims] Migrations applied successfully
[uxmClaims] uxmClaims enabled!
```

### Step 4: Check the Config Files

After the first start, uxmClaims creates its configuration files:

```
plugins/
└── uxmClaims/
    ├── config.yml        ← Main settings
    ├── messages.yml      ← All text messages
    ├── roles.yml         ← Role definitions
    ├── entitlements.yml  ← Limits and costs
    ├── webhooks.yml      ← Discord notifications
    └── menu/             ← Menu layouts (27 files)
```

---

## That's It! 🎉

The plugin is now running with default settings. Players can type `/claim` to open the main menu.

---

## What's Next?

---

## Common Installation Problems

### "Plugin failed to load"

**Cause:** Your Java version is too old.

**Fix:** Install Java 21 or newer. Check with:

```bash
java -version
```

### "Database connection failed"

**Cause:** If using MySQL/PostgreSQL, the connection settings are wrong.

**Fix:** For now, use SQLite (the default). It works without any setup. See [Database Setup](../database/sqlite.md) later.

### "Players can't use /claim"

**Cause:** Missing permissions.

**Fix:** Players need no special permission by default. Check if another permission plugin is blocking it.
