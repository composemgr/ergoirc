## 👋 Welcome to ergoirc 🚀

Self-hosted IRC server with a web client

## 📋 Description

Self-hosted IRC server with a web client. Ergo provides the IRC daemon and
The Lounge provides the browser-based web client, which is the only service
published on the host.

## 🚀 Services

- **app**: thelounge/thelounge:latest

### Infrastructure Components

- **server**: ghcr.io/ergochat/ergo:latest


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/ergoirc/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/ergoirc" ~/.local/srv/docker/ergoirc
cd ~/.local/srv/docker/ergoirc
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install ergoirc
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=IRC
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64165` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64165

## 📂 Volumes

- `./volumes/data/ergoirc` - The Lounge user and client configuration
- `./volumes/config/ergoirc` - Ergo IRC server configuration


## 🔐 Security

- Set `APP_ORG_NAME` to the IRC network name clients should see
- The Ergo daemon listens on 6667 inside the project network only; publish it
  on the host yourself if external IRC clients need to connect
- The `172.17.0.1` bind address limits access to the Docker host gateway only
- Set up HTTPS using a reverse proxy (nginx, traefik, caddy) for production
- Keep regular backups of the data and config volumes

## 🔍 Logging

```shell
docker compose --env-file .env logs -f
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
