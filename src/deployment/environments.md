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
go build -o auth .
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

## Production server cheatsheet

These commands are only meant to be run when you are logged in on the production server as the `backend` user.

### Check deployment status

```bash
cd /home/backend/dodeka/deploy
uv run dodeka-status.py
```

### Get demo admin credentials

```bash
cd /home/backend/dodeka/backend
uv run --frozen --no-dev ba --env demo get-admin-credentials
```

### Restart demo services

```bash
~/dodeka/deploy/restart-demo.sh
```

### Restart production services

```bash
~/dodeka/deploy/restart-production.sh
```

### View logs for a failing service

```bash
journalctl -u dodeka-backend-demo
```

### Manually run a database backup

```bash
~/dodeka/deploy/dodeka-db-backup.sh demo
```

### Enable SMTP in production

```bash
~/dodeka/deploy/enable-smtp.sh production smtp-relay.gmail.com
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

## Board admin account

The real board admin account is the **Bestuur** account, hardcoded to `bestuur@dsavdodeka.nl`. The `root_admin@localhost` account from `get-admin-credentials` is only a bootstrap account, not the account the board should use.

Setting it up is a two-step flow. Run these from `~/dodeka/backend` on the server, with `--env demo` or `--env production` as appropriate.

**1. Create the account and send the signup email:**

```bash
uv run --frozen --no-dev ba --env production board-setup
```

This creates an accepted registration for `bestuur@dsavdodeka.nl`, marks it as a system user, and sends a signup email. It does **not** grant admin yet.

**2. After signup completes** (the board sets a password via the signup email), grant admin:

```bash
uv run --frozen --no-dev ba --env production grant-admin bestuur@dsavdodeka.nl
```

This adds the `admin` permission and marks the account as a system user.

### Yearly board handover

For a new board, do **not** run `board-setup` again. Use `board-renew`, which resets the password (sends a reset email) and re-grants admin on the same `bestuur@dsavdodeka.nl` account:

```bash
uv run --frozen --no-dev ba --env production board-renew
```

### Making any other user an admin

`grant-admin` works on any email, so to promote an existing user (after they have signed up) to admin, point it at their address:

```bash
uv run --frozen --no-dev ba --env production grant-admin someone@example.com
```

> Demo can send real email too — run `~/dodeka/deploy/enable-smtp.sh demo <smtp-relay>` (and restart the demo backend) to set `BACKEND_SMTP_SEND=true`. If SMTP is not enabled, no mail is delivered; in that case complete a signup manually by fetching the token with `ba --env demo get-token signup bestuur@dsavdodeka.nl`.

## Setting up from scratch

See [Setting up from scratch](./production/server_scratch.md).
