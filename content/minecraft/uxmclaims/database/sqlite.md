---
title: SQLite (Easy)
order: 380
---

## Why SQLite?

| Pros                            | Cons                                           |
|---------------------------------|------------------------------------------------|
| ✅ No setup required             | ❌ Not ideal for very large servers             |
| ✅ Works out of the box          | ❌ Single-file, harder to back up while running |
| ✅ Fast for small-medium servers | ❌ No remote access                             |
| ✅ No external software          |                                                |

**Recommended for:** Most servers, especially those with \<100 concurrent players

---

## Configuration

In `config.yml`:

```yaml
database:
  type: 'AUTO'
  username: 'root'       # Not used for SQLite
  password: 'password'   # Not used for SQLite
  url: 'jdbc:sqlite:./plugins/uxmClaims/claims.db'
```

That's it! The plugin creates the database file automatically.

---

## Database Location

The database file is created at:

```
plugins/uxmClaims/claims.db
```

This single file contains all claim data.

---

## Backing Up

To back up your data:

1. **Stop the server** (recommended for clean backup)
2. Copy `claims.db` to a safe location
3. Start the server

You can also copy while running, but there's a small risk of corruption.

---

## Migrating to MySQL/PostgreSQL

If your server grows, you might want to migrate:

1. Export data using external tools
2. Change database settings in config.yml
3. Import data to new database
4. Restart server

---

## Troubleshooting

### "Database is locked"

- Only one process should access the file
- Restart the server
- Check for leftover lock files

### "File not found"

- The plugin creates the file automatically
- Check permissions on the plugins folder

### Performance issues

If you have many claims and notice slowdowns:

- Consider upgrading to MySQL/PostgreSQL
- See [MySQL Setup](../database/mysql.md)
