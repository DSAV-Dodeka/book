# Migrate database schema

First, log in to the production server and enter the `dodeka` repository.

Then, be sure you have a GitHub token ready from an account with access to the `backend` repository, with at least
`read:org` and `repo` scope (preferably the DodekaComCom account).

Then, from the main repo directory, run:

```shell
./use/data_sync/migrate_env.sh
```

This will prompt you for the token. Paste it in and press enter. A new `migrate` directory will have appeared, which has been copied from the `backend` repository (the `src/schema` package, to be precise).

Now, run alembic:

```shell
poetry run alembic revision --autogenerate -m "<Some migration message>"
```

For troubleshooting, refer to the [backend setup docs](/development/setup_backend.md).