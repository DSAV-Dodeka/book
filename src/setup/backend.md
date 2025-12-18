# Backend setup

## OUTDATED: not yet correct for new backend

**NOTE**: the backend is now developed from the `dodeka` repository, in the `backend` subdirectory!

Before you can run the backend locally, you must have a Postgres and Redis database setup. Take a look at the [database setup](./setup_docker.md) page for that.

**Run all commands from the `dodeka/backend` folder!**

* [Install `uv`](https://github.com/astral-sh/uv?tab=readme-ov-file#installation). uv is like npm, but then for Python. I recommend using the standalone installer.
* Then, set up a Python environment. Use `uv python install` inside the `./backend` directory, which should then install the right version of Python, or use one you already have installed.
* Next, sync the project using `uv sync --group dev` (to also install dev dependencies). This will also set up a virtual environment at `./.venv`.
* Then I recommend connecting your IDE to the environment. In the previous step `uv` will have created a virtual environment in a .venv directory. Point your IDE to that executable (the file named `python` or `python.exe` in `.venv/bin`) to make it work.
* Currently, the `apiserver` package is in a /src folder which is nice for test isolation, but it might confuse your IDE (if you use PyCharm). In that case, find something like 'project structure' configuration and set the /src folder as a 'sources folder' (or similar, might be different in your IDE).
* You might want some different configuration options. Maybe, you want to test sending emails or have the database connected somewhere else. In that case, you probably want to edit your `devenv.toml`, which contains important config options. However, this means that when you push your changes to Git, everyone else will get your version. If their are secret values included, those will be publicly available on Git as well! Instead, create a copy of `devenv.toml` called `devenv.toml.local` and make changes there. Now, Git will know to ignore this file.
* Now you can run the server either by just running the `dev.py` in src/apiserver (easiest if you use PyCharm) or by running `uv run backend` (easiest if you use VS Code). The server will automatically reload if you change any files. It will tell you at which address you can access it.

### Running for the first time

If you are running the server for the first time and/or the database is empty, be sure to set RECREATE="yes" in the env config file (i.e. `devenv.toml.local`). Be sure to set it back to "no" after doing this once, or otherwise it recreates it each time.
