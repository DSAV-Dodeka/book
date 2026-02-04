# Backups

The database is backed up using [Restic](https://restic.net/), which provides incremental, encrypted, deduplicated backups. Each backup is compressed with zstd before being stored. Because Restic deduplicates across snapshots, frequent backups are storage-efficient.

The backup scripts live in `deploy/` in the dodeka repository. See `deploy/BACKUP.md` for the full reference.

## How it works

1. The SQLite database is exported with `VACUUM INTO` (produces a clean, compacted copy)
2. The export is compressed with `zstd --rsyncable`
3. The compressed file is uploaded to a Restic repository at `/mnt/backup/restic`

Each snapshot is tagged (e.g. `db_dodeka`, `env_production`) so you can filter when listing or restoring.

## Setup

See [server from scratch](./production/server_scratch.md) how to set it up on the server.

## Setting up the cron job

The cron script (`dodeka-db-cron.sh`) runs a backup and then prunes old snapshots according to the retention policy. A pre-made crontab file is provided that backs up both production and demo every 12 minutes (with a 6-minute offset so they don't overlap).

Install it with:

```bash
crontab ~/dodeka/deploy/dodeka_crontab
```

This replaces your current crontab. The schedule is:
- **Production**: every 12 minutes (`:00`, `:12`, `:24`, `:36`, `:48`)
- **Demo**: every 12 minutes at 6-minute offset (`:06`, `:18`, `:30`, `:42`, `:54`)

Output is appended to `/home/backend/log/backup.log`.

### Retention policy

The cron script automatically prunes old snapshots with the following retention:

| Keep | Count |
|------|-------|
| Latest | 10 |
| Hourly | 24 |
| Daily | 7 |
| Weekly | 4 |
| Monthly | 6 |

## Manual backup

```bash
~/dodeka/deploy/dodeka-db-backup.sh production
~/dodeka/deploy/dodeka-db-backup.sh demo
```

## Restore

```bash
# Restore from same environment
~/dodeka/deploy/dodeka-db-restore.sh production

# Clone production data to demo
~/dodeka/deploy/dodeka-db-restore.sh demo --from production
```

The restore script will:
1. List available snapshots and ask you to pick one (or type `latest`)
2. Stop the backend service
3. Back up the current database (tagged `pre-restore`, so you can undo)
4. Download and decompress the selected snapshot
5. Replace the database file (clearing WAL/SHM files)
6. Restart the service

## Listing snapshots

```bash
# All dodeka snapshots
restic -r /mnt/backup/restic --password-file /mnt/backup/.restic-password snapshots --tag db_dodeka

# Only production
restic -r /mnt/backup/restic --password-file /mnt/backup/.restic-password snapshots --tag env_production
```
