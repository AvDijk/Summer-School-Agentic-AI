<div align="center">

# n8n + FastAPI + ngrok Stack

Automated workflow platform (n8n) + example FastAPI service, exposed securely over the internet via a reserved ngrok domain.

</div>

---
## 1. Features
* n8n (automation/workflows) reachable locally & publicly
* FastAPI demo service (health + root endpoints)
* ngrok tunnel (reserved domain) with simple config
* SQLite persistence (optional future PostgreSQL)
* Clean dependency management via `uv` + `pyproject.toml`

---
## 2. Architecture
```
┌──────────┐    docker net     ┌─────────┐
│  ngrok   │ ───────────────▶  │  n8n     │
└──────────┘                   │(5678)   │
       │                       └─────────┘
 Public URL                         │
       │                            │shares volume
       ▼                            ▼
  Internet                     SQLite DB (data/)
                                     ▲
                     ┌───────────┐   │
                     │ FastAPI   │───┘ (8000)
                     └───────────┘
```

---
## 3. Prerequisites
| Requirement | Notes |
|-------------|-------|
| Docker / Docker Compose v2 | Required to run the stack |
| ngrok account | Needed for auth token + reserved domain |
| (Optional) sqlite3 client | For local DB inspection |

---
## 4. Clone Repository
```bash
git clone <your-repo-url> && cd n8n_test
```

---
## 5. Environment Setup
1. Copy the example env file:
   ```bash
   cp .env.example .env
   ```
2. Edit values in `.env`:
   - `SUBDOMAIN` → your reserved ngrok subdomain (without domain suffix)
   - `DOMAIN_NAME` → typically `ngrok-free.app`
   - `N8N_AUTHTOKEN` → from https://dashboard.ngrok.com/get-started/your-authtoken
   - (Optional) uncomment `N8N_ENCRYPTION_KEY` for production
3. Ensure the domain in `n8n.yaml` matches: `SUBDOMAIN.DOMAIN_NAME`

---
## 6. ngrok Configuration
File: `n8n.yaml`
```yaml
version: "2"
log_level: warn
tunnels:
  n8n:
    proto: http
    addr: n8n:5678
    domain: <SUBDOMAIN>.<DOMAIN_NAME>
```
The compose file mounts this as `/etc/ngrok.yml` in the ngrok container.

---
## 7. Build & Start
```bash
docker compose up -d --build
```
Watch logs (optional):
```bash
docker compose logs -f n8n
```

---
## 8. Service URLs
| Service | URL |
|---------|-----|
| n8n UI | http://localhost:5678 |
| Public (ngrok) | https://<SUBDOMAIN>.<DOMAIN_NAME> |
| FastAPI | http://localhost:8000 |
| ngrok Dashboard | http://localhost:4040 |

Replace placeholders with actual values once `.env` is set.

---
## 16. Clean Removal
```bash
docker compose down -v
rm -rf data files
```