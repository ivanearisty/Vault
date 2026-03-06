# Building Security Camera

> Package stolen from the building. Residents agreed to let me install a Tapo C110 in the hallway ceiling. The thief may be a resident who requests access — system must be hardened.

---

## Phase 1: Hardware & Camera Setup

- [ ] Mount the Tapo C110 on the hallway ceiling
- [ ] Connect it to the building's WiFi (or my own AP if needed)
- [ ] Set up the camera via the Tapo app — configure RTSP access
- [ ] Note the camera's local IP, RTSP port (554), username, password, and stream path
- [ ] Verify RTSP works: open `rtsp://user:pass@<ip>:554/stream1` in VLC from my laptop

---

## Phase 2: Build the `building-security` Repo

### 2a. Scaffold the repo

- [ ] Create `/Users/suape/WorkDir/building-security/` with the full directory structure:
  ```
  building-security/
  ├── recorder/        (Dockerfile, record.sh, cleanup.sh)
  ├── web-api/         (Dockerfile, nginx.conf, auth.py, requirements.txt)
  ├── web/             (Next.js frontend)
  ├── docker-compose.yml
  ├── .env.example
  ├── .gitignore
  ├── pyproject.toml
  └── .github/workflows/ci.yml
  ```
- [ ] `git init` and push to GitHub as `building-security`

### 2b. Recorder container

- [ ] Write `recorder/Dockerfile` — base `alpine:3.19`, install ffmpeg via apk
- [ ] Write `recorder/record.sh` — ffmpeg RTSP → HLS segments, organized by date (`/recordings/YYYY-MM-DD/`)
- [ ] Write `recorder/cleanup.sh` — deletes folders older than 14 days
- [ ] Entrypoint: starts ffmpeg + cron for cleanup at midnight
- [ ] Test locally: `docker build` and run with my camera's RTSP URL, confirm `.ts` segments appear

### 2c. Web API container

- [ ] Write `web-api/auth.py` — FastAPI app:
  - `POST /auth/login` — username/password → JWT (24hr expiry)
  - `GET /auth/me` — validate token
  - `GET /api/days` — list available recording dates
  - `GET /api/download?date=...&start=...&end=...` — concatenate `.ts` → mp4 via ffmpeg (max 1hr)
  - Admin-only: `POST /auth/users`, `POST /auth/users/{id}/disable`
  - CLI commands: `create-admin`, `create-user`, `disable-user`, `reset-password`
  - SQLite (`/data/users.db`) — bcrypt 12 rounds, admin flag, enabled flag
  - Audit log every request to `/data/audit.log`
- [ ] Write `web-api/requirements.txt` — fastapi, uvicorn, pyjwt, bcrypt, python-multipart
- [ ] Write `web-api/nginx.conf` — hardened:
  - Allowlisted routes only (`/auth/login`, `/auth/me`, `/api/days`, `/api/download`, `/recordings/**/*.ts`, `/recordings/**/*.m3u8`), everything else → 403
  - Only GET and POST allowed
  - `autoindex off`, `server_tokens off`, `client_max_body_size 1k`
  - Rate limits: 10 req/s on auth, 50 req/s on video segments
  - Security headers: `X-Content-Type-Options`, `X-Frame-Options DENY`, CSP, `Referrer-Policy`
  - CORS: only allow the Vercel frontend origin
  - JWT validation via `auth_request` subrequest for `/recordings/` paths
- [ ] Write `web-api/Dockerfile` — base `python:3.11-slim`, install nginx, copy config + app
- [ ] Test locally: build and run, confirm auth endpoints work with curl

### 2d. Next.js Frontend

- [ ] Scaffold `web/` with Next.js + Tailwind
- [ ] `/` (login page) — username + password form → calls `/auth/login` → stores JWT in localStorage
- [ ] `/player` (protected) — redirects to `/` if no valid JWT
  - Date picker populated from `/api/days`
  - hls.js video player loads `<tunnel-url>/recordings/YYYY-MM-DD/playlist.m3u8`
  - Download button: pick start/end time → calls `/api/download`
- [ ] `lib/auth.ts` — JWT token management (store, retrieve, attach to requests, check expiry)
- [ ] Set `NEXT_PUBLIC_API_URL` env var for the backend URL
- [ ] Test with `npm run dev` against the local web-api container

### 2e. CI/CD

- [ ] Write `.github/workflows/ci.yml`:
  - On PR: lint with ruff on `web-api/auth.py`
  - On push to main: build + push `recorder` and `web-api` images to `ghcr.io/ivanearisty/`
- [ ] Push to GitHub and verify CI runs green

---

## Phase 3: Cloudflare Tunnel

- [ ] Create a free Cloudflare account (or use existing) at https://dash.cloudflare.com
- [ ] Add my domain (or use a free `*.trycloudflare.com` subdomain)
- [ ] Install `cloudflared` on TrueNAS (`apt install cloudflared` or run as container)
- [ ] Create the tunnel: `cloudflared tunnel create building-security`
- [ ] Configure it to route to `http://localhost:8443` (the security-api nginx port)
- [ ] Run the tunnel: `cloudflared tunnel run building-security`
- [ ] Note the public URL (e.g., `building-security.mydomain.com`)

---

## Phase 4: Deploy to TrueNAS

### 4a. Update homelab-infra

- [ ] Add `security-recorder` and `security-api` services to `homelab-infra/docker-compose.yml`
  - `security-recorder`: image from ghcr.io, volume `security_recordings:/recordings`
  - `security-api`: image from ghcr.io, port `8443:80`, volumes `security_recordings:/recordings:ro` + `security_data:/data`
  - Both: `env_file: ./services/building-security/config.env`, Watchtower labels
- [ ] Create `homelab-infra/services/building-security/config.env` with:
  - `CAMERA_HOST`, `CAMERA_PORT`, `CAMERA_USERNAME`, `CAMERA_PASSWORD`, `CAMERA_STREAM_PATH`
  - `JWT_SECRET` (generate a strong random secret)
- [ ] Commit and push homelab-infra changes

### 4b. Bring it up

- [ ] SSH into TrueNAS
- [ ] `docker compose pull && docker compose up -d`
- [ ] Verify both containers are healthy: `docker compose ps`
- [ ] Create admin account:
  ```
  docker compose exec security-api python auth.py create-admin admin <password>
  ```
- [ ] Start the Cloudflare tunnel (if not already running as a service)

---

## Phase 5: Deploy Frontend to Vercel

- [ ] Go to https://vercel.com → New Project → Import `building-security` from GitHub
- [ ] Set root directory to `web/`
- [ ] Add env var: `NEXT_PUBLIC_API_URL=https://building-security.mydomain.com`
- [ ] Deploy — auto-deploys on push to main going forward
- [ ] Update nginx CORS config to allow the Vercel URL origin

---

## Phase 6: Create Resident Accounts

- [ ] On TrueNAS, create accounts for each resident:
  ```
  docker compose exec security-api python auth.py create-user resident1 TempPassword123
  ```
- [ ] Give each resident their username + password (in person or via DM)
- [ ] Tell them: no self-registration, no password reset — DM me if they forget

---

## Phase 7: Verification

### Functional Tests

- [ ] Confirm ffmpeg is writing `.ts` segments to `/recordings/YYYY-MM-DD/`
- [ ] Open a `.m3u8` playlist in VLC — video plays
- [ ] `curl -X POST https://<tunnel>/auth/login -d '{"username":"...","password":"..."}'` → get JWT back
- [ ] `curl https://<tunnel>/recordings/...m3u8` without JWT → 401
- [ ] `curl https://<tunnel>/recordings/...m3u8 -H "Authorization: Bearer <token>"` → 200
- [ ] Open the Vercel frontend → log in → player loads video with hls.js
- [ ] Test download: pick a time range, confirm mp4 downloads

### Security Tests

- [ ] `curl -X DELETE https://<tunnel>/recordings/2026-03-03/` → 403
- [ ] `curl -X PUT https://<tunnel>/auth/users` → 403
- [ ] `curl https://<tunnel>/etc/passwd` → 403
- [ ] `curl https://<tunnel>/recordings/../../../etc/passwd` → 403 (path traversal blocked)
- [ ] Rapid-fire wrong passwords on `/auth/login` → rate limit kicks in
- [ ] Non-admin user calls `POST /auth/users` → 403
- [ ] Check `/data/audit.log` — all access attempts are logged with timestamp + IP

---

## Architecture Diagram

```
Tapo C110 (RTSP)
    │
    ▼
┌─────────────────┐     ┌──────────────────┐
│  recorder        │     │  web-api          │
│  (ffmpeg → HLS)  │────▶│  (nginx + FastAPI)│
│                  │     │                   │
│  /recordings/    │     │  JWT auth         │
└─────────────────┘     │  Audit log        │
                         │  Static HLS serve │
                         └────────┬──────────┘
                                  │ :8443
                                  ▼
                         Cloudflare Tunnel
                                  │
                                  ▼
                         Vercel (Next.js)
                         Login + HLS Player
```

---

## What Residents Can Do

- Log in with credentials I gave them
- View live-ish footage (~10s HLS delay)
- Browse recordings by date
- Download clips (max 1 hour)

## What Residents Cannot Do

- Create/modify accounts or see other users
- List directories or access paths outside allowlisted routes
- Upload anything
- Make PUT/DELETE/PATCH requests
- Access without a valid JWT
- Brute force logins (rate limited)
- Hit the server directly (Cloudflare Tunnel only)
