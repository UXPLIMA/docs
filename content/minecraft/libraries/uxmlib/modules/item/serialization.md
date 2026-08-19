---
title: Serialization
order: 224
description: Turning an item into bytes that survive every data component, and migrating items written by an older server.
icon: file-archive
---

Storing an item means storing every one of its data components, including the ones your code has
never heard of. Anything that reconstructs an item field by field loses whatever it did not know to
copy.

```java
byte[] bytes = ItemSerialization.toBytes(sword);
ItemStack back = ItemSerialization.fromBytes(bytes);

String base64 = ItemSerialization.toBase64(sword);
ItemStack fromText = ItemSerialization.fromBase64(base64);
```

Use the byte form for a `BLOB` column, and the base64 form for a text column or a config file.

## Compressed

For a container full of items, gzip pays for itself.

```java
byte[] small = ItemSerialization.toCompressedBytes(sword);
ItemStack back = ItemSerialization.fromCompressedBytes(small);

String text = ItemSerialization.toCompressedBase64(sword);
ItemStack item = ItemSerialization.fromCompressedBase64(text);
```

## Data versions and migration

An item serialized on an older server carries an older Minecraft data version. Deserializing it
directly can fail or silently lose data.

```java
OptionalInt version = ItemSerialization.dataVersionOf(bytes);

if (ItemMigrations.needsMigration(bytes)) {
    byte[] current = ItemMigrations.migrate(bytes);
}

ItemStack migrated = ItemMigrations.migrateItem(bytes);
```

`migrateItem` is the one-call form: read, upgrade if needed, and hand back an item on the current
version.

<Callout type="tip" title="Migrate on read, write on the current version">

Run stored blobs through `migrateItem` when you load them and write them back with `toBytes`. A
vault loaded once after a server upgrade is then permanently current, and the migration path stays
short instead of accumulating another hop each release.

</Callout>
