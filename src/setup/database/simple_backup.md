# Simple backup

Ensure you are in the `dodeka/deploy` directory.

To create a backup, run:
```shell
uv run psqlsync --config data/test.toml --action backup
```

If the backup is running with a password, use the `--prompt-pass` option. You can then type/copy-paste the database password.

```shell
uv run psqlsync --config data/test.toml --action backup --prompt-pass
```

From your own computer, you can then use ssh copy to transfer the file.

```shell
scp backend@<ip address>:/home/backend/dodeka/data/backups/backup-20230430-154532-dodeka.dump.gz <destination>
```