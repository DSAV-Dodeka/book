# Environments

The [`dodeka` repository](https://github.com/DSAV-Dodeka/dodeka) builds Docker containers (the `build/container` directory) and builds the scripts for deploying them (`build/deploy`). Both are differentiated based on the "environment" or "mode" of deployment. We distinguish the following:

* 'production' mode
* 'staging' mode
* 'test' mode (not yet in use)
* 'localdev' mode

There is also 'envless', for when you run tests without actually spinning up the entire application.

The DB (database, PostgreSQL) and KV (key-value store, Redis) are designed to vary very little depending on their mode, accepting simple configuration options and allowing to be wrapped by simple scripts to handle different modes.

The Server and Pages have more significant differences between modes.

In general, modes are pre-selected for deploy builds, but for container builds they are selected at build time (in CI). Furthermore, deploy builds can generally be run locally, while container builds are run in CI to ensure reproducability. 