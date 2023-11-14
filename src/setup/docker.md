# Docker/container setup

We use the `dodeka` repository for the setup of the database and key-value store (a special database that is not relational, it's basically a big dictionary/map). For this, we use a technology called containers. Specifically, we use Docker.

In order to run the scripts, there are a few requirements. If you're on macOS or Linux, you only need to install Docker as both are Unix-like systems. For macOS I recommend only installing Docker Engine

First of all, you need to have a Unix-like command line with a bash-compatible shell: i.e. Linux or macOS. See the Notion for instructions on Windows Subsystem for Linux (**WSL**), which allows you to install Linux inside Windows.

Then, you need a number of tools installed, which you can install from the links below if you're not on Windows:

* [Docker Engine](https://docs.docker.com/engine/install/)

If you're on Windows, installing [Docker Desktop](https://www.docker.com/products/docker-desktop) after you've installed WSL will make these available inside WSL if Docker Desktop is running.

### Dev

To be able to run everything, you need to have configured access to the containers. To do that, run:

```shell
docker login ghcr.io
```

Enter your GitHub username. For the password, don't use your GitHub password, but a Personal Access Token (Settings -> Developer settings) with at least read:packages and write:packages permissions. Be sure to save the token somewhere safe, you'll probably have to reuse it and you can't view it in GitHub after creation!

Now, you will need to be able to access the scripts in this repository. If you're using Windows, **do not** copy the files from Windows to Linux, this leads to some weird formatting problems in the scripts that cause them to fail. Instead, clone this repository directly from WSL, by running:

`git clone https://github.com/DSAV-Dodeka/dodeka.git`

You will again need to enter your GitHub username and the Personal Access Token.

You will now have a `dodeka` folder containing all the necessary folders.

In `/dev` you can find `devdeploy.sh` and `down.sh`. By running `./devdeploy.sh` you start both the database and key-value store.

If you're deploying from WSL, run `devedeploy.sh port` instead. This is because if your container is running inside WSL, for it to be accessible from outside WSL, the host needs to be 0.0.0.0 instead of localhost.

#### Troubleshooting

The following error can occasionally occur:

```
cp: cannot create regular file './conf/redis.conf': Permission denied
./kv/deploy.sh: line 28: ./conf/redis.conf: Permission denied
```

If this happens, simply delete the `conf` directory in `use/dev/kv` and retry.