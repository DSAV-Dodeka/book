# Test database


#### Syncing the test database

A number of test databases are stored inside the `DSAV-Dodeka/backend` repository. Running the commands above creates an empty database. To populate it with the latest test values, run:

```shell
poetry run python -c "from use.data_sync.cli import run; run()"
```

You will probably need to set the GHMOTEQLYNC_DODEKA_GH_TOKEN as an environment variable for access. The safest way to set this is to add it to a file like `sync.env`:

```shell
export GHMOTEQLYNC_DODEKA_GH_TOKEN="GitHub Personal Access Token"
```

Here you should replace "GitHub Personal Access Token" with the value of your token, which will need `repo` scope. You can then run `. sync.env` before running the script to sync the database.