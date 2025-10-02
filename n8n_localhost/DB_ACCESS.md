# SQLite Database Guide

Database file location:
```
data/database.sqlite
```

## Open Interactive Shell
```bash
sqlite3 data/database.sqlite
```

## List Tables
```sql
.tables
```

## Inspect Schema
User table:
```sql
.schema user
```

Any table:
```sql
.schema table_name_here
```

## Query Users
```sql
SELECT id, email, password FROM user LIMIT 10;
```

## Update a User Password
(Generate a bcrypt hash first.)
```sql
UPDATE user
SET password = 'REPLACE_WITH_BCRYPT_HASH',
    updatedAt = STRFTIME('%Y-%m-%d %H:%M:%f', 'NOW')
WHERE email = 'you@example.com';
```

## Backup Before Changes
```bash
sqlite3 data/database.sqlite ".backup 'data/backup_before_change.sqlite'"
```

## Exit
```sql
.quit
```

## Safety Tips
- Always back up before destructive changes.
- Never store plaintext passwords.
- Rotate weak credentials after testing.
