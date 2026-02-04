# Backend setup

The backend is developed from the [dodeka](https://github.com/dsav-dodeka/dodeka) repository, in the `backend` folder. 

**Run all commands from the `dodeka/backend` folder!**

* [Install `uv`](https://github.com/astral-sh/uv?tab=readme-ov-file#installation). uv is like npm, but then for Python. I recommend using the standalone installer.
* Ensure you have `gh` (GitHub CLI) installed and authenticated (`gh auth login`). See [Git setup](./git.md) for instructions. This is needed to auto-download the auth binary.
* Then, set up a Python environment. Use `uv python install` inside the `./backend` directory, which should then install the right version of Python, or use one you already have installed.
* Next, sync the project using `uv sync --frozen`. This will also set up a virtual environment at `./.venv`.
* Then I recommend connecting your IDE to the environment. In the previous step `uv` will have created a virtual environment in a .venv directory. Point your IDE to that executable (the file named `python` or `python.exe` in `.venv/bin`, ask an LLM or look at uv's README for more details) to make it work.
* Now run `uv run dev` inside the `dodeka/backend` folder. This will start two programs:
  * The `auth` server, based on [faroe](https://github.com/faroedev/faroe/) through [tiauth-faroe](https://github.com/tiptenbrink/tiauth-faroe)
  * The Python backend itself, which builds on top of the very basic HTTP server [freetser](https://github.com/tiptenbrink/freetser)
* On startup, you will see something like:
```
[Thread-6 (run_startup_actions)] [backend] Root admin bootstrapped: root_admin@localhost (user_id=7_root_admin)
[Thread-6 (run_startup_actions)] [backend] Root admin credentials: email=root_admin@localhost, password=yttU06VMelR5d7MU4bVt65RcSlmnOGSkIn0Qt0vQWaA
```
You can use those credentials to log in as admin through `http://localhost:3000` (assuming that's where your frontend is running).
