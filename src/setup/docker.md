# Docker/container setup

We use the `dodeka` repository for the setup of the database and key-value store (a special database that is not relational, it's basically a big dictionary/map). For this, we use a technology called containers. Specifically, we use Docker.

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

Those commands are quite long and hard to remember. To make things easier, the `dodeka` project has a `justfile`, which provides some command shorcuts. First, install [`just`](https://just.system) on the platform you are running your commands from (I recommend WSL if on Windows). This is a "command runner". I recommend to install it using `cargo` (which you can also install... via https://rustup.rs/), so simply run `cargo install just`. 

Then, we have the following commands that you can now run when inside the `dodeka` repository: 

to start
```
just updevp
```

to stop
```
just downdevp
```


Remove the `p`, so:

```
just updev

just downdev
```

if you are not on WSL.


