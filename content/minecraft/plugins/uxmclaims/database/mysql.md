---
title: MySQL
order: 390
description: Creating the database and user, and pointing the plugin at MySQL.
icon: database
---

## Why MySQL?

| Pros                            | Cons                             |
|---------------------------------|----------------------------------|
| ✅ Good for large servers        | ❌ Requires external setup        |
| ✅ Remote access possible        | ❌ More complex configuration     |
| ✅ Better concurrent performance | ❌ Needs separate backup strategy |
| ✅ Easy to query/modify data     |                                  |

**Recommended for:** 100+ concurrent players, network servers

---

## Requirements

- MySQL 8.0 or newer
- Or MariaDB 10.5 or newer

---

## Step 1: Create Database

Connect to MySQL and run:

```sql
CREATE DATABASE uxmclaims;
CREATE USER 'uxmclaims'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON uxmclaims.* TO 'uxmclaims'@'localhost';
FLUSH PRIVILEGES;
```

Replace `your_secure_password` with a strong password.

---

## Step 2: Configure Plugin

In `config.yml`:

```yaml
database:
  type: 'AUTO'
  username: 'uxmclaims'
  password: 'your_secure_password'
  url: 'jdbc:mysql://localhost:3306/uxmclaims'
```

### URL Format

```
jdbc:mysql://HOST:PORT/DATABASE
```

| Part     | Default   | Example       |
|----------|-----------|---------------|
| HOST     | localhost | 192.168.1.100 |
| PORT     | 3306      | 3306          |
| DATABASE | uxmclaims | uxmclaims     |

---

## Step 3: Restart Server

Restart your Minecraft server. Check the console for:

```
[uxmClaims] Connected to MySQL database
[uxmClaims] Migrations applied successfully
```

---

## Remote MySQL

For remote databases (like a separate database server):

```yaml
url: 'jdbc:mysql://192.168.1.100:3306/uxmclaims'
```

Make sure:

- The MySQL server allows remote connections
- Firewall allows port 3306
- MySQL user is created for your server's IP

---

## Connection Options

You can add options to the URL:

```yaml
url: 'jdbc:mysql://localhost:3306/uxmclaims?useSSL=false&allowPublicKeyRetrieval=true'
```

Common options:

- `useSSL=false` - Disable SSL (for local connections)
- `allowPublicKeyRetrieval=true` - Fix caching_sha2_password issues
- `autoReconnect=true` - Reconnect if connection drops

---

## Troubleshooting

### "Access denied"

- Check username and password
- Make sure the user has permissions on the database
- Try connecting with the MySQL client first

### "Unknown database"

- Create the database first (Step 1)
- Check spelling

### "Connection refused"

- Is MySQL running?
- Is the port correct?
- Is the host correct?
- Check firewall settings

### "Public key retrieval not allowed"

Add to URL:

```
?allowPublicKeyRetrieval=true
```

---

## Backing Up

```bash
mysqldump -u uxmclaims -p uxmclaims > backup.sql
```

Restore:

```bash
mysql -u uxmclaims -p uxmclaims \< backup.sql
```
