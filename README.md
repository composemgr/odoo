## 👋 Welcome to Odoo 🚀

Business apps for sales, inventory, accounting and manufacturing

## 📋 Description

Odoo is an all-in-one business application suite covering CRM, sales,
inventory, accounting, manufacturing, website and e-commerce. The official
container image serves the web interface and the background worker from the
same container, so this stack ships the app plus a PostgreSQL database.

Odoo stores its filestore and addons under `/var/lib/odoo` and its
configuration under `/etc/odoo`. Both are bind mounted so state survives an
image change.

## 🚀 Services

- **app**: odoo:latest (web interface, published)

### Infrastructure Components

- **db**: Postgres database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/odoo/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/odoo" ~/.local/srv/docker/odoo
cd ~/.local/srv/docker/odoo
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install odoo
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ORG_NAME=Odoo
DB_USER_NAME=odoo
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=odoo
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
| `docker-compose.yaml` | Default, published on `172.17.0.1:64276` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service carries the proxy labels and joins the external
network. The database stays on the internal project network and is never
published.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64276

## 📂 Volumes

- `./volumes/data/odoo` - Odoo filestore, addons and session data
- `./volumes/config/odoo` - Odoo configuration directory
- `./volumes/db/postgres/odoo` - Database data


## 🔐 Security

- Change all default passwords before deploying to production
- The database is only reachable on the internal project network, do not
  publish it
- Odoo is served over plain HTTP by default; use HTTPS via one of the reverse
  proxy variants in production
- Terminate TLS before Odoo when running behind a proxy, and set
  `web.base.url` in the Odoo settings to the public URL
- Regularly update Docker images for security patches
- Backup both the filestore and the database regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
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

# Install a module from the addons path
docker compose --env-file .env exec app odoo -d odoo -i module_name --stop
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- At least 2 GB of memory

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
