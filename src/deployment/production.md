

# Staging and production

For a complete setup including backend, first ensure the containers are built using GitHub Actions for the environment you want to deploy. Then, SSH into the cloud server you want to deploy it to. First, ensure Python, Docker (including Compose), gpg, GitHub CLI and bash are installed on the target server. Next, log into GitHub CLI with an account with access to this repository and the `dodekasecrets` repository.

To deploy, simply clone this repository and enter the main directory. Make sure you have updated the repository recently with the newest deploy script versions. Then, run `./deploy.sh production` for production. If you replace "production" with "staging", by default it will reset the database, so be careful! To deploy the staging version without reset, run `./deploy.sh staging update`.

It will ask you for the passphrase of `dodekasecrets`. Paste it in and press enter, the rest will then happen automatically!

That's it!

#### How to shutdown

After running deploy, it will create a new directory in `deployments/active<environment>` and move the scripts of the right environment's `use` folder. This will contain all scripts for the deployment, so you can also run `./down.sh` from there. In case there is a crash in one of the containers, do this so the startup will be clean.