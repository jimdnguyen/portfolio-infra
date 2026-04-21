# portfolio-infra

Production hosting config for jimdnguyen.dev — Caddy reverse proxy + Docker Compose.

## Structure

```
~/projects/
├── portfolio-infra/   ← this repo
├── finally/
├── pm/
├── prelegal/
└── site/
```

## Services

| URL | Service |
|-----|---------|
| jimdnguyen.dev | SolidJS static site |
| finally.jimdnguyen.dev | AI trading workstation (FastAPI + SSE) |
| pm.jimdnguyen.dev | Project manager (FastAPI) |
| prelegal.jimdnguyen.dev | Pre-legal tool (FastAPI) |

## Deploy

### First time

```bash
# Clone all repos
cd ~/projects
git clone https://github.com/jimdnguyen/portfolio-infra
git clone https://github.com/jimdnguyen/finally
git clone https://github.com/jimdnguyen/pm
git clone https://github.com/jimdnguyen/prelegal
git clone https://github.com/jimdnguyen/site

# Build static site
cd ~/projects/site && npm install && npm run build
cp -r dist/ ~/projects/portfolio-infra/site-dist/

# Create .env files (never committed)
nano ~/projects/finally/.env
nano ~/projects/pm/.env
nano ~/projects/prelegal/.env

# Start everything
cd ~/projects/portfolio-infra
docker compose -f docker-compose.prod.yml up -d
```

### Update a service

```bash
cd ~/projects/finally && git pull
cd ~/projects/portfolio-infra && docker compose -f docker-compose.prod.yml up -d --build finally
```

### Logs

```bash
docker compose -f docker-compose.prod.yml logs -f
docker compose -f docker-compose.prod.yml logs caddy
```

## Prerequisites

- Oracle Cloud free tier VM (Ubuntu 22.04)
- Tailscale installed on VM and laptop
- SSH locked to Tailscale IP in Oracle security list
- Cloudflare managing DNS for jimdnguyen.dev (SSL mode: Full strict)
- Docker installed on VM
- fail2ban installed on VM
