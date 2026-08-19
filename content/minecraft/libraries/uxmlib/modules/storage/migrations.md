---
title: Migrations
order: 253
description: Versioned schema migrations that run once, plus introspection and column operations for the cases they cannot express.
icon: git-compare-arrows
---

## Versioned migrations

```java
new MigrationRunner(db).apply(List.of(
        new Migration(1, "init", "CREATE TABLE warps (name TEXT PRIMARY KEY)"),
        new Migration(2, "add world", "ALTER TABLE warps ADD COLUMN world TEXT")));
```

Each migration runs exactly once, in version order, and the applied version is recorded. Leaving this
block in `onEnable` forever is safe and is the intended use.

```java
int applied = new MigrationRunner(db).apply(migrations);
int version = new MigrationRunner(db).currentVersion();
```

## Migrations as resources

Keeping SQL in `.sql` files rather than Java strings makes it reviewable and diffable.

```java
List<Migration> migrations = MigrationResources.load(getClassLoader(), "migrations");
new MigrationRunner(db).apply(migrations);
```

```java
List<Migration> effective = MigrationResources.overlay(
        MigrationResources.load(loader, "migrations"),
        MigrationResources.load(loader, "migrations/mysql"));
```

`overlay` replaces a base migration with a backend-specific one of the same version, for the cases
where the dialects genuinely need different SQL.

## Introspection

Sometimes you need to know what is there before deciding what to do.

```java
SchemaIntrospector schema = new SchemaIntrospector(db);

schema.tableExists("warps");
schema.columnExists("warps", "world");
schema.ensureColumn("warps", "world", "TEXT");
schema.dialect();
```

`ensureColumn` adds the column if it is missing and returns whether it did. It is the idempotent
form, useful for a plugin adopting a table it did not create.

## Column operations

SQLite in particular does not support every `ALTER TABLE` other backends do, so these smooth over the
difference.

```java
SchemaOps ops = new SchemaOps(db);

ops.dropColumn("warps", "legacy");
ops.renameColumn("warps", "owner", "owner_uuid");
ops.alterColumnType("players", "coins", SqlType.bigint());
ops.alterColumnType("players", "coins", "BIGINT");

int copied = ops.copyColumnData("warps", "owner", "owner_uuid");
int wiped = ops.wipeColumnData("warps", "legacy");
int reset = ops.resetTable("sessions");
```

The typical rename-with-migration shape is: add the new column, copy the data across, then drop the
old one, each step idempotent so a half-finished upgrade can be resumed.

<Callout type="warning" title="These are not transactional across backends">

A column operation on SQLite may be implemented as a table rebuild. Take a backup before a
destructive schema change, and prefer a numbered `Migration` over an ad-hoc `SchemaOps` call for
anything that ships to users.

</Callout>
