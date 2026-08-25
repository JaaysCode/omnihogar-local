# omnihogar-local

Full stack: `omnihogar-backend` (.NET 10 Web API + PostgreSQL via EF Core) and `omnihogar-frontend` (Angular 22 SSR), wired together with Docker Compose.

## Run it

```bash
git submodule update --init --recursive
cp .env.example .env   # fill in JWT_SECRET at least
docker compose up --build
```

- Frontend: http://localhost:4200
- Backend API + Swagger (dev only): http://localhost:5244/swagger
- Postgres: localhost:5432 (db `omnihogar`, user `postgres`)

Backend can apply EF Core migrations automatically on startup (`Database.Migrate()` in `Program.cs`), gated by `APPLY_MIGRATIONS_ON_STARTUP` (default `false`). It's off by default because the current `InitialCreate` migration is starter scaffold (ASP.NET Identity tables + a `Product` entity), not the real data model. DB comes up empty. Once you've designed your model and generated your own migration(s), set `APPLY_MIGRATIONS_ON_STARTUP=true` in `.env`.

## Services

| Service  | Image / build              | Port (host) |
|----------|-----------------------------|-------------|
| db       | `postgres:17-alpine`        | 5432        |
| backend  | `omnihogar-backend/Dockerfile` | 5244     |
| frontend | `omnihogar-frontend/Dockerfile` | 4200    |

Data persists in the `db-data` named volume across restarts. `docker compose down -v` wipes it.
