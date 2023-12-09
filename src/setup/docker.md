# Docker/container setup

We use the `deploy` folder in the `dodeka` repository for the setup of the database and key-value store (a special database that is not relational, it's basically a big dictionary/map). For this, we use a technology called containers. Specifically, we use Docker.

In order to run the scripts, there are a few requirements. If you're on macOS or Linux, you only need to install Docker as both are Unix-like systems. For macOS I recommend only installing Docker Engine

First of all, you need to have a Unix-like command line with a bash-compatible shell: i.e. Linux or macOS. See the Notion for instructions on Windows Subsystem for Linux (**WSL**), which allows you to install Linux inside Windows.

Then, you need a number of tools installed, which you can install from the links below if you're not on Windows:

* [Docker Engine](https://docs.docker.com/engine/install/)

If you're on Windows, installing [Docker Desktop](https://www.docker.com/products/docker-desktop) after you've installed WSL will make these available inside WSL if Docker Desktop is running.

## Dev

To be able to run everything, you need to have configured access to the containers. To do that, run:

```shell
docker login ghcr.io
```

Enter your GitHub username. For the password, don't use your GitHub password, but a Personal Access Token (Settings -> Developer settings) with at least read:packages and write:packages permissions. Be sure to save the token somewhere safe, you'll probably have to reuse it and you can't view it in GitHub after creation!

Now, you will need to be able to access the scripts in this repository. If you're using Windows, **do not** copy the files from Windows to Linux, this leads to some weird formatting problems in the scripts that cause them to fail. Instead, clone this repository directly from WSL, by running:

`git clone https://github.com/DSAV-Dodeka/dodeka.git`

You will again need to enter your GitHub username and the Personal Access Token.

You will now have a `dodeka` folder containing all the necessary folders.

Now, we will use Docker Compose so start everything we want for development:

First, we pull:

```shell
docker compose -f use/dev/docker-compose.yml --env-file use/dev/dev.env --profile data pull
```

We start:

```shell
docker compose -f use/dev/docker-compose.yml --env-file use/dev/dev.env --profile data up -d
```

We shutdown:

```shell
docker compose -f use/dev/docker-compose.yml --env-file use/dev/dev.env --profile data down
```

(To understand this command, we are basically indicating three options. `-f` is which compose file we are running, `--env-file` is which environment variables we are setting and `--profile` which services. We choose `data` because we only want to run the databases.)

Note that they will only be accessible on localhost from the environment they run in. So if you are using WSL, they are only available from localhost. So if the backend project is stored in Windows, you need to change the `--env-file` option to `use/dev/dev_port.env`. This will open the databases on the 0.0.0.0 host, which will make them accessible even from outside WSL.

## Shortcuts

Those commands are quite long and hard to remember. To make things easier, we use Nu shell scripts to provide shorter commands that are easier to remember. First, follow the instructions on [installing nu below](#installing-nushell).

On Windows, you need to prepend `nu` to every command. For convenience, the commands below all contain `nu`. However, on Linux/macOS this is not necessary, you can just run the script directly once you've installed Nushell (because they recognize shebangs).

The scripts are all in the root directory, but you can call them using `../` if you are in a subdirectory and they'll still work. Make sure [you've installed Nu](#installing-nushell).

### Running the development databases

**Start** (this will also pull the images, so make sure you're logged in with `docker login ghcr.io`):

Note: this opens the ports on host 0.0.0.0, so only do this when your Docker doesn't run on your main OS, so use this if it e.g. runs inside WSL

```
nu dev.nu upp
```

**Stop**:

```
nu dev.nu down
```

If running Docker on your main OS:

```
nu dev.nu up
```

### Testing and checking the backend

```
nu test.nu backend
```

Other commands can be found in the various `.nu` files in the root directory.

### Installing Nushell

Install [`nu`](https://www.nushell.sh/book/installation.html) (you don't have to set it as your default shell, just make sure you have `nu` on your path).

If you have Rust installed, you can install it using `cargo install nu`. This is especially recommended if you're on Linux (to install it from a binary, first run `cargo install binstall` and then `cargo binstall nu`). On Windows, the easiest is probably `winget install nushell` (using [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/)). On macOS, install it using Homebrew (`brew install nushell`).

Why Nushell? In short, because it is a modern shell scripting language that lets you very easily call external programs. Its syntax is also readable by people who haven't used it before, but still quite powerful. It also can replace all kinds of different tools we might need otherwise.

