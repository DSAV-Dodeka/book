

# Staging and production

For a complete setup including backend, first ensure the containers are built using GitHub Actions for the environment you want to deploy. Then, SSH into the cloud server you want to deploy it to. 

The following tools are necessary, in addition to those assumed to be installed on a standard Linux server:

* Python 3.10 (matching the version requirements of the `dodeka` repository)
* Poetry (to install the dependencies in the `dodeka` repository)
* `gh` (GitHub CLI, logged into account with access to `dodeka` repository, optional if authenticated by other means)
* `nu` (a shell scripting language, see also [how to install](../setup/docker.md#installing-nushell).)
* `tidploy` ([tool](https://github.com/tiptenbrink/tidploy) built specifically for deploying this project)
* `bws` ([Bitwarden Secrets Manager CLI](https://github.com/bitwarden/sdk))

The last three tools are all Rust projects, so they can be built from source using `cargo install <tool name>`. However, this can be very slow on large VMs, so installing them as binaries using `cargo binstall <tool name>` is recommended (install `binstall` first by using `cargo install cargo-binstall`).

To deploy, simply clone this repository and enter the main directory. Make sure you have updated the repository recently with the newest deploy script versions. Then, run `tidploy auth deploy` and enter the Bitwarden Secrets Manager access token. 

Then, you can deploy using `tidploy deploy production` (or `tidploy deploy staging` for staging). You can also use the Nu shortcut:

```
nu deploy.nu create production
```


If you want to specify the specific Git tag (i.e. a release or commit hash) to deploy, use:

```
nu deploy.nu create staging v2.1.0-rc.3
```

That's it!

### Shutdown

You can observe the active compose project using `docker compose ls`. Then you can shut it down by running (from the `dodeka` repository):

```
nu deploy.nu down production
```


If the suffix of the Docker Compose project name is different from latest, replace it with that. It will in general be equal to the tag you deployed with (which is 'latest' by default), but with periods replaced with underscores. For example, `nu deploy.nu create staging v2.1.0-rc.3` can be shutdown with `nu deploy.nu down staging v2_1_0-rc_3`.