# Backend setup

Before you can run `apiserver` locally, you must have a Postgres and Redis database setup. Take a look at the [database setup](./setup_docker.md) page for that.

* [Install Poetry](https://python-poetry.org/docs/master/). Poetry is like npm, but then for Python.
* Then, set up a Python 3.11 Poetry virtual environment. This step can be complicated, as Python 3.11 might not be the default version of Python on your system. First, install 3.11. There are multiple ways to do that. For advanced users (if you frequently use multiple versions of Python), you could use something like `pyenv`. Then, to make Poetry use the correct version, find the path of the Python executable (so the python.exe or similar) of your Python 3.11 installation. Then, do `poetry env use <python3.11 path here>`.
* Then I recommend connecting your IDE to the environment. The best way is to simply run `poetry update`. It will give you a path towards the virtualenv it created, which will contain a python executable in the /bin folder (if it doesn't, run `poetry env info`). If you point your IDE to that executable as the project interpreter, everything should work.
* Next run `poetry install`, which will also install the project. Currently, the `apiserver` package is in a /src folder which is nice for test isolation, but it might confuse your IDE. In that case, find something like 'project structure' configuration and set the /src folder as a 'sources folder' (or similar, might be different in your IDE).
* Before running, you must have the environment variable APISERVER_CONFIG set to `./devenv.toml` in order to be able to run it. This can be most easily done by editing the run configuration of your IDE to always set that environment variable. (If you ask why, this is to force you to consider to which database, etc. you are connecting.) There are multiple other ways to set it. If you are on Linux, you can run `export APISERVER_CONFIG=./devenv.toml` if you run it from the terminal. On Windows, you can edit your system environment variables and add it there. Do note that this will set it everywhere and that might lead to issues, so if possible set it in the run configuration!
* Now you can run the server either by just running the `dev.py` in src/apiserver or by running `poetry run s-dodeka`. The server will automatically reload if you change any files. It will tell you at which address you can access it.

### Running for the first time

If you are running the server for the first time and/or the database is empty, be sure to set RECREATE="yes" in the env config file (i.e. `devenv.toml`). Be sure to set it back to "no" after doing this once, or otherwise it recreate it each time.

### Database migrations

We can use Alembic for migrations, which allow you to programatically apply large schema changes to your database.

First you need to have the Poetry environment running as described earlier and ensure the database is on as well. 

* Navigate to the ./src/schema directory.
* From there run `poetry run alembic revision --autogenerate -m "Some message"`
* This will generate a Python file in the migrations/versions directory, which you can view to see if everything looks good. It basically looks at the database, looks at the schema described in db/model.py and generates code to migrate to the described schema.
* Then, you can run `poetry run alembic upgrade head`, which will apply the latest generated revision. If you now use your database viewer, the table will have hopefully appeared.
* If there is a mismatch with the current revision, use `poetry run alembic stamp head` before the above 2 commmands.