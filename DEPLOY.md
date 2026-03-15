# Render Deployment

## Service Details

| Field | Value |
|-------|-------|
| Service ID | `srv-d6r1jnq4d50c73blia4g` |
| URL | https://express-hello-world-pc54.onrender.com |
| Dashboard | https://dashboard.render.com/web/srv-d6r1jnq4d50c73blia4g |
| Region | Oregon |
| Plan | Free |
| Runtime | Node |
| Auto-deploy | Yes (on push to `main`) |

## How It Works:

### App

A minimal Express server (`app.js`) that serves an HTML page on `/`. It listens on `process.env.PORT` (Render sets this automatically) or falls back to `3001` locally.

### Render Configuration

`render.yaml` defines the service as a Blueprint:

```yaml
services:
  - type: web
    name: express-hello-world
    runtime: node
    plan: free
    buildCommand: yarn install --frozen-lockfile
    startCommand: node app.js
    envVars:
      - key: NODE_ENV
        value: production
```

### Deployment Flow

1. Push to `main` on GitHub
2. Render detects the push (auto-deploy enabled)
3. Render runs `yarn install --frozen-lockfile` (build step)
4. Render runs `node app.js` (start step)
5. Service is live at the `.onrender.com` URL

## Render CLI

### Setup

```sh
render workspace set tea-csps9si3esus73cqfab0
```

### Useful Commands

```sh
# List services
render services list

# Check deploy status via API
curl -s -H "Authorization: Bearer $RENDER_API_KEY" \
  https://api.render.com/v1/services/srv-d6r1jnq4d50c73blia4g/deploys

# Trigger a manual deploy
curl -s -X POST -H "Authorization: Bearer $RENDER_API_KEY" \
  https://api.render.com/v1/services/srv-d6r1jnq4d50c73blia4g/deploys

# Get deploy logs
curl -s -H "Authorization: Bearer $RENDER_API_KEY" \
  https://api.render.com/v1/services/srv-d6r1jnq4d50c73blia4g/deploys/{deploy-id}/logs
```

### Deploy Statuses

| Status | Meaning |
|--------|---------|
| `created` | Deploy queued |
| `build_in_progress` | Building |
| `update_in_progress` | Starting the service |
| `live` | Successfully deployed |
| `deactivated` | Shut down |
| `build_failed` | Build step failed |
| `update_failed` | Start step failed |
| `canceled` | Manually canceled |

### API Key

Stored in `~/.render/cli.yaml`. The CLI authenticates automatically once configured. For direct API calls, pass the key as a Bearer token.
