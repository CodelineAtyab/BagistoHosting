# Bagisto Production Setup (Cloudflare Tunnel)

## Architecture
```
User (HTTPS) → Cloudflare → cloudflared → localhost:6000 → Nginx → PHP-FPM (Bagisto)
                                                                  ↕
                                                            MySQL + Redis
```

## First Time Setup

### 1. Clone and configure
```bash
git clone <your-repo> bagisto-prod
cd bagisto-prod
cp .env.example .env
# Edit .env — fill in all passwords and APP_URL
```

### 2. Start containers
```bash
docker compose up -d
```

### 3. Generate APP_KEY
```bash
docker compose exec bagisto php artisan key:generate
```

### 4. Install Bagisto
```bash
docker compose exec bagisto php artisan bagisto:install
```

### 5. Verify all services are healthy
```bash
docker compose ps
```

---

## Day-to-Day Commands

| Task | Command |
|---|---|
| Start | `docker compose up -d` |
| Stop | `docker compose down` |
| View logs | `docker compose logs -f` |
| Bagisto logs only | `docker compose logs -f bagisto` |
| Run artisan | `docker compose exec bagisto php artisan <cmd>` |
| Clear cache | `docker compose exec bagisto php artisan optimize:clear` |
| DB shell | `docker compose exec db mysql -u bagisto_user -p bagisto` |
| Redis shell | `docker compose exec redis redis-cli -a $REDIS_PASSWORD` |

---

## Production Checklist

- [ ] `.env` has strong unique passwords
- [ ] `APP_KEY` is generated and saved
- [ ] `APP_URL` matches your Cloudflare domain (https://)
- [ ] `.env` is in `.gitignore` — never committed
- [ ] Cloudflare tunnel URL points to `http://localhost:6000`
- [ ] Cloudflare SSL mode set to **Full** in dashboard
