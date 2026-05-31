# Environments

Each environment (demo, production) runs two systemd services: an auth server (`dodeka-auth-{env}`) and a Python backend (`dodeka-backend-{env}`). Both need to be started separately, but they connect to each other automatically. Always start/restart auth before the backend.

## Service Names

| Environment | Auth service | Backend service |
|---|---|---|
| Demo | `dodeka-auth-demo` | `dodeka-backend-demo` |
| Production | `dodeka-auth-production` | `dodeka-backend-production` |

## Files

Environment files live in:

- `dodeka/backend/envs/demo/.env`
- `dodeka/backend/envs/production/.env`
- `dodeka/backend/auth/envs/demo/.env`
- `dodeka/backend/auth/envs/production/.env`

Database files live next to those environment files.

## Operations

Common deployment commands now live on the main [Deployment](../deployment.md) page.

For backups and restore, see [Backups](./backups.md).

For a new server, see [Setting up from scratch](./production/server_scratch.md).
