# Database

Everything is deployed from the `dodeka` repository. It also contains the "source" for the database (PostgreSQL) and key-value store (Redis).

The most important file is `config.toml`, which contains all practical configuration. In the `build`-folder you can find the source for all deploy scripts (`build/deploy`) and container build files (`build/container`). Using the `confspawn` tool the actual scripts are built from these templates. The results you can find in the various folders in the `use` directory.


## Building the scripts and containers

* [Poetry](https://python-poetry.org/docs/master/)
    * Once installed, run `poetry update` inside the main directory. This will install the other requirements.

### Deploy scripts

Building the deployment scripts is easy, simply run `build_deploy.sh` in the main directory.

### Containers

The containers have dedicated GitHub Actions workflows to build them, so in general you should never have to build them locally. Take a look at the workflows to see how they are built.
