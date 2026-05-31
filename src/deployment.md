# Deployment

This is the starting point for running and updating the deployed backend.

Use these commands only on the production server as the `backend` user.

## Quick Status

```bash
cd /home/backend/dodeka/deploy
uv run dodeka-status.py
```

For systemd directly:

```bash
sudo systemctl status dodeka-auth-demo dodeka-backend-demo
sudo systemctl status dodeka-auth-production dodeka-backend-production
```

## Deploy Backend Changes

```bash
cd ~/dodeka
git pull
```

If Python dependencies changed:

```bash
cd ~/dodeka/backend
uv sync --frozen --no-dev
```

If auth code changed:

```bash
cd ~/dodeka/backend/auth
go build -o auth .
```

Restart the right environment:

```bash
~/dodeka/deploy/restart-demo.sh
~/dodeka/deploy/restart-production.sh
```

## Logs

Follow logs:

```bash
journalctl -u dodeka-auth-demo -u dodeka-backend-demo -f
journalctl -u dodeka-auth-production -u dodeka-backend-production -f
```

Recent logs:

```bash
journalctl -u dodeka-auth-demo -u dodeka-backend-demo --since "1 hour ago"
```

## Start And Stop

Auth should start before backend.

```bash
sudo systemctl stop dodeka-backend-demo dodeka-auth-demo
sudo systemctl start dodeka-auth-demo dodeka-backend-demo

sudo systemctl stop dodeka-backend-production dodeka-auth-production
sudo systemctl start dodeka-auth-production dodeka-backend-production
```

## Demo Admin

```bash
cd /home/backend/dodeka/backend
uv run --frozen --no-dev ba --env demo get-admin-credentials
```

## Reset Demo

This wipes demo auth and backend databases.

```bash
~/dodeka/deploy/reset-demo.sh
```

## Manual Backup

```bash
~/dodeka/deploy/dodeka-db-backup.sh demo
~/dodeka/deploy/dodeka-db-backup.sh production
```

For restore, see [Backups and restore](./deployment/backups.md).

## Board Admin Account

The real board admin account is `bestuur@dsavdodeka.nl`. The `root_admin@localhost` account is only a bootstrap account.

First-time setup:

```bash
cd ~/dodeka/backend
uv run --frozen --no-dev ba --env production board-setup
```

After the board completes signup:

```bash
uv run --frozen --no-dev ba --env production grant-admin bestuur@dsavdodeka.nl
```

Yearly handover:

```bash
uv run --frozen --no-dev ba --env production board-renew
```

Grant admin to another existing user:

```bash
uv run --frozen --no-dev ba --env production grant-admin someone@example.com
```

## SMTP

```bash
~/dodeka/deploy/enable-smtp.sh production smtp-relay.gmail.com
```

Demo can send real email too:

```bash
~/dodeka/deploy/enable-smtp.sh demo smtp-relay.gmail.com
```

If SMTP is disabled, fetch a signup token manually:

```bash
cd ~/dodeka/backend
uv run --frozen --no-dev ba --env demo get-token signup bestuur@dsavdodeka.nl
```

## More

- [Server access](./deployment/server.md)
- [Environment structure](./deployment/environments.md)
- [Backups and restore](./deployment/backups.md)
- [Server setup from scratch](./deployment/production/server_scratch.md)
