# Odoo

A self-hosted odoo application.

## Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/odoo/main/docker-compose.yaml" | docker compose -f - up -d
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/odoo" ~/.local/srv/docker/odoo
cd ~/.local/srv/docker/odoo
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install odoo
```

## Configuration

See docker-compose.yaml for environment variables and configuration options.

## Documentation

Check the official project documentation for detailed setup and usage information.
