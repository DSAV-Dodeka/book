# Environments

Each environment (demo, production) runs two systemd services: an auth server (`dodeka-auth-{env}`) and a Python backend (`dodeka-backend-{env}`). Both need to be started separately, but they connect to each other automatically. Always start/restart auth before the backend.

## Deploying updates

After pulling the latest code on the server:

```bash
cd ~/dodeka
git pull
```

Rebuild the auth binary (if auth code changed):

```bash
cd ~/dodeka/backend/auth
CGO_ENABLED=1 go build -o auth .
```

Sync Python dependencies (if they changed):

```bash
cd ~/dodeka/backend
uv sync --frozen --no-dev
```

Then restart the services using the helper scripts in `deploy/`:

```bash
~/dodeka/deploy/restart-demo.sh
~/dodeka/deploy/restart-production.sh
```

## Checking status

```bash
sudo systemctl status dodeka-auth-demo dodeka-backend-demo
sudo systemctl status dodeka-auth-production dodeka-backend-production
```

## Viewing logs

Follow logs in real time:

```bash
journalctl -u dodeka-auth-demo -u dodeka-backend-demo -f
journalctl -u dodeka-auth-production -u dodeka-backend-production -f
```

Show recent logs:

```bash
journalctl -u dodeka-auth-demo -u dodeka-backend-demo --since "1 hour ago"
```

## Stopping and starting

```bash
# Stop both
sudo systemctl stop dodeka-backend-demo dodeka-auth-demo

# Start both (auth first)
sudo systemctl start dodeka-auth-demo dodeka-backend-demo
```

## Resetting demo

To wipe the demo databases (both auth and backend) and start fresh:

```bash
~/dodeka/deploy/reset-demo.sh
```

This stops the demo services, deletes all database files, and starts the services again.

Alternatively, restore from a backup (see [Backups](./backups.md)).

## Setting up from scratch

See [Setting up from scratch](./production/server_scratch.md).
