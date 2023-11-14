# Database

Everything is deployed from the `dodeka` repository. It also contains the "source" for the database (DB, PostgreSQL) and key-value store (KV, Redis).

The most important file is `config.toml`, which contains all practical configuration. In the `build`-folder you can find the source for all deploy scripts (`build/deploy`) and container build files (`build/container`). Using the `confspawn` tool the actual scripts are built from these templates. The results you can find in the various folders in the `use` directory.

### Note on `confspawn`

The total configuration was spread over a lot of different files in different places (PostgreSQL config, various Docker Compose files...). Some configuration is also very project-dependent (names like 'dodeka'). To keep things more generable and have one single source of truth for the configuration, Tip developed a Python tool called [`confspawn`](https://github.com/tiptenbrink/confspawn) which can take in a configuration "template" (where certain values are not filled in yet) and fill them in using a secondary configuration source. It uses Jinja2 templates for this.



