## 1. Build & Start All Services
```bash
docker compose up -d --build
```

## 2. Access Services
- n8n UI: http://localhost:5678
- Public (ngrok): https://precious-peacock-boss.ngrok-free.app
- FastAPI: http://localhost:8000
- ngrok Dashboard: http://localhost:4040

## 3. Stop Everything
```bash
docker compose down
```

## 4. Full Reset (removes data)
```bash
docker compose down -v
rm -rf data/* files/*
```

## 5. Database Guide
See `DB_ACCESS.md` for SQLite usage (opening the DB, listing tables, updating user passwords, taking backups).