# Backend development

## OUTDATED: not yet accurate for new backend

## Project structure

All the application code is inside the `backend/src` directory. This contains five separate packages, of which three act as libraries and two as applications.

- The `datacontext` library is fully standalone. It contains special logic for implementing dependency injection, which is useful for replacing database-reliant functions in tests, while keeping good developer ergonomics. Ensure it doesn't import code from any other package!
- The `store` library is fully standalone and provides the primitives for communicating with the databases (both DB and KV). Ensure it doesn't import code from any other package!
- The `auth` library relies on both the datacontext and store libraries. It provides an application-agnostic implementation of all the authorization server logic. In an ideal world, the authorization server is a separate application. To still stay as close to this as possible, we develop it as a separate library. However, the library does not know about HTTP or anything like that, the routes are implemented in our actual implementation, as are some things which rely on a specific schema.
- The `schema` package contains the definition of our database schema (in `schema/model/model.py`). It can be extracted during deployment and then used for applying migrations, hence it is also something of an application. 
- The `apiserver` package is our actual FastAPI application. It relies on all the above four packages. However, it also has some internal logic that is more "library"-like. Furthermore, to prevent circular imports among other things, there is a certain "dependency order" we want to keep. They are as follows:
    - `resources.py` contains two variables that make it easier to get the specific path, specifically import files in the `resources` folder.
    - `define.py` contains a number of constants that are unlikely to ever change and do not really depend on what environment the application is deployed in (whether it is development, staging, production, etc.). It also contains the logic for loading things that do depend on the 'general' environment, but not the 'local' environment. As a rule of thumb, something like a website URL will always be the same for an environment, but an IP address, a port or a password might differ.
    - `env.py` loads this local configuration, which includes things like passwords and where to exactly find the database. 
    - Then we have the `src/apiserver/lib` module, which consists mostly of logic that does not load its own data. While it might _cause_ side effects (like sending an email), it should always cause the same side effects for the same arguments (so it should not load data). In general, most functions and logic here should be pure. More importantly, they should not import anything from the `src/apiserver/app` module.
    - Next there is `src/apiserver/data`. This include all the simple functions that perform a single action relating to external data (so the DB or KV). Mostly, these functions wrap `store` functions, but then using a specific table or schema. The most important are the functions in the `data/api`, i.e. the data "API" which is the way that the rest of the application interacts with data. Inside `data/context` it also contains context functions, which should call multiple `data/api` functions and other effectful code that you wan to easily replace in test (like generating something randomly). See `data/context/__init__.py` for more details.
    - Finally, we come to `src/apiserver/app`. These contain the most critical part, namely the `routers`, which define the actual API endpoints. Furthermore, there is the `modules` module, which mostly wrap multiple context functions. See `app/modules/__init__.py` for more details.
    - Next, the `app_...` files define and instantiate the actual application, while `dev.py` is an entrypoint for running the program in development.  

## Other

### Important to keep in mind

Always add a trailing "/" to endpoints.

### Testing

We have a number of tests in the `tests` directory. To run them and check if you didn't break anything important, you can run `poetry run pytest`.

### Static analysis and formatting

To improve code quality, readability and catch some simple bugs, we use a number of static analysis tools and a formatter. We use the following:

* `mypy` checks if our type hints check out. Run using `poetry run mypy`. This is the slowest of all the tools.
* `ruff` is a linter, so it checks for common mistakes, unused imports and other simple things. Run using `poetry run ruff src tests actions`. To automatically fix issues, add `--fix`.
* `black` is a formatter. It ensures we never have to discuss formatting mistakes, we just let the tool handle it for us. You can use `poetry run black src tests actions` to run it.

You can run all these tools at once using the `Poe` taskrunner, by running the following in the terminal:

```shell
poe check
```

### Continuous Integration (CI)

Tests (including some additional tests that run against a live database) and all the above tools are all run in GitHub actions. If you open a Pull Request, these checks are run for every commit you push. If any fail, the "check" will fail, indicating that we should not merge. 

### VS Code settings

VS Code doesn't come included with all necessary/useful tools for developing a Python application. Therefore, be sure the following are installed:

* Python (which installs Pylance)
* Even Better TOML (for .toml file support)

You probably want to update `.vscode/settings.json` as follows:

```json
{
    "python.analysis.typeCheckingMode": "basic",
    "files.associations": {
        "*.toml.local": "toml"
    },
    "files.exclude": {
        "**/__pycache__": true,
        "**/.idea": true,
        "**/.mypy_cache": true,
        "**/.pytest_cache": true,
        "**/.ruff_cache": true
      }
}
```

This ensures that any unnecessary and files are not shown in the Explorer.
