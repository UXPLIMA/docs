---
title: PostgreSQL
order: 400
icon: database
---

## Why PostgreSQL?

| Pros                             | Cons                               |
|----------------------------------|------------------------------------|
| ✅ Excellent performance at scale | ❌ More complex setup               |
| ✅ Advanced JSON support          | ❌ Requires more resources          |
| ✅ Great for complex queries      | ❌ Less common in Minecraft hosting |
| ✅ Robust data integrity          |                                    |

**Recommended for:** Enterprise deployments, database experts

---

## Requirements

- PostgreSQL 13.0 or newer

---

## Step 1: Create Database

Connect to PostgreSQL and run:

```sql
CREATE DATABASE uxmclaims;
CREATE USER uxmclaims WITH ENCRYPTED PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE uxmclaims TO uxmclaims;
```

Also grant schema permissions:

```sql
\c uxmclaims
GRANT ALL ON SCHEMA public TO uxmclaims;
```

---

## Step 2: Configure Plugin

In `config.yml`:

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: 'your_secure_password'
  url: 'jdbc:postgresql://localhost:5432/uxmclaims'
```

### URL Format

```
jdbc:postgresql://HOST:PORT/DATABASE
```

| Part     | Default   | Example       |
|----------|-----------|---------------|
| HOST     | localhost | 192.168.1.100 |
| PORT     | 5432      | 5432          |
| DATABASE | uxmclaims | uxmclaims     |

---

## Step 3: Restart Server

Restart your Minecraft server. Check the console for:

```
[uxmClaims] Connected to PostgreSQL database
[uxmClaims] Migrations applied successfully
```

---

## Connection Options

```yaml
url: 'jdbc:postgresql://localhost:5432/uxmclaims?ssl=false'
```

Common options:

- `ssl=false` - Disable SSL for local connections
- `sslmode=require` - Require SSL for remote connections

---

## Troubleshooting

### "Password authentication failed"

- Check username and password
- Verify pg_hba.conf allows your connection method

### "Connection refused"

- Is PostgreSQL running?
- Check port 5432
- Verify postgresql.conf allows network connections

### "Permission denied"

- Run the GRANT commands from Step 1
- Make sure user has schema permissions

---

## Backing Up

```bash
pg_dump -U uxmclaims uxmclaims > backup.sql
```

Restore:

```bash
psql -U uxmclaims uxmclaims \< backup.sql
```

---

## Which Database Should You Use?

| Server Size                | Recommendation      |
|----------------------------|---------------------|
| Small (\<50 players)        | SQLite              |
| Medium (50-200 players)    | MySQL or SQLite     |
| Large (200+ players)       | MySQL or PostgreSQL |
| Network (multiple servers) | MySQL or PostgreSQL |
