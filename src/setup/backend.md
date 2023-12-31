# Backend setup

**NOTE**: the backend is now developed from the `dodeka` repository, in the `backend` subdirectory!

Before you can run the backend locally, you must have a Postgres and Redis database setup. Take a look at the [database setup](./setup_docker.md) page for that.

**Run all commands from the `dodeka/backend` folder!**

* [Install Poetry](https://python-poetry.org/docs/master/). Poetry is like npm, but then for Python. It also provides better project isolation, leveraging virtual environments better than most other projects. 
* Then, set up a Python 3.11 Poetry virtual environment. This step can be complicated, as Python 3.11 might not be the default version of Python on your system. First, install 3.11. There are multiple ways to do that. For advanced users (if you frequently use multiple versions of Python), you could use something like `pyenv`. Then, to make Poetry use the correct version, find the path of the Python executable (so the python.exe or similar) of your Python 3.11 installation. Then, do `poetry env use <python3.11 path here>`.
* Then I recommend connecting your IDE to the environment. The best way is to simply run `poetry update`. It will give you a path towards the virtualenv it created, which will contain a python executable in the /bin folder (if it doesn't, run `poetry env info`). If you point your IDE to that executable as the project interpreter, everything should work.
* Next run `poetry install`, which will also install the project. Currently, the `apiserver` package is in a /src folder which is nice for test isolation, but it might confuse your IDE. In that case, find something like 'project structure' configuration and set the /src folder as a 'sources folder' (or similar, might be different in your IDE).
* You might want some different configuration options. Maybe, you want to test sending emails or have the database connected somewhere else. In that case, you probably want to edit your `devenv.toml`, which contains important config options. However, this means that when you push your changes to Git, everyone else will get your version. If their are secret values included, those will be publicly available on Git as well! Instead, create a copy of `devenv.toml` called `devenv.toml.local` and make changes there. Now, Git will know to ignore this file.
* Now you can run the server either by just running the `dev.py` in src/apiserver (easiest if you use PyCharm) or by running `poetry run backend` (easiest if you use VS Code). The server will automatically reload if you change any files. It will tell you at which address you can access it.

### Running for the first time

If you are running the server for the first time and/or the database is empty, be sure to set RECREATE="yes" in the env config file (i.e. `devenv.toml.local`). Be sure to set it back to "no" after doing this once, or otherwise it recreates it each time.