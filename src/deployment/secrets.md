# Secrets

## Passwords
- Hetzner account studentenatletiek@av40.nl
- root user password of the server
- backend user password of the server
- Everything in `dodekasecrets` (Postgres password, Redis password, server secret) and the passphrase to encrypt these secrets
- Symmetric encryption key for refresh tokens and asymmetric signing key for access and ID tokens (stored in database)