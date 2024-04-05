# Setting up the server from scratch

This is all the commands I used when I set up the server from a clean image on January 10 after we migrated the email to the new provider. 

### Preparing SSH

First we rebuild the image from Ubuntu 22.04.

We reset the root password using Rescue -> Reset root password. I recommend then changing it to a new password once inside again (through `passwd root`).

Login using the console GUI.

Go to `/etc/ssh` and change the SSH server settings `sshd_config`:

```
PermitRootLogin no
PasswordAuthentication no
```

Then we create a new user (-m creates home directory, then we add them to sudo group):

```
useradd -m backend
adduser backend sudo
```

Then do `passwd backend` and set up a password.

Switch to the user using `su backend`. 

The default shell might not be bash (for example if your prompt starts with only '$'), in that case run:

```
chsh --shell /bin/bash
```

Create a new directory `mkdir /home/backend/.ssh`. Enter this directory (using `cd`) and then do `nano authorized_keys` to open/create a new file there.

Paste in your SSH public key (!) (something like `ssh-ed25519 .... tiptenbrink@tipc`) and save the file (Ctrl-X).

Then, ensure the file has the correct permissions:

```
chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
```

### SSH niceties

Install "xauth" (while logged in as root)

```
apt install xauth
```

Then, login to backend (`su backend`) and log out again. If you're using a nice terminal emulator, like kitty, you need to add the xterm file to the server. To do this, one time append `kitty +kitten` before your ssh command like:

```
kitty +kitten ssh backend@<ip here>
```

After that you can just login normally. Other terminal emulators might need other instructions.

**From this point on we never need to be logged in as root anymore!**

### Update packages

```
sudo apt update
sudo apt upgrade
```

You might have to reboot after this: `reboot`.

### Install basic C ompiler

```
sudo apt install build-essential
```

### Install Rust

Go to https://rustup.rs/

Run the listed script. Choose "Customize", then profile "minimal".

### Install cargo binary install tools

```
cargo install cargo-quickinstall
cargo quickinstall cargo-binstall
```

### Install nu, tidploy

```
cargo binstall nu
cargo binstall tidploy
```

### Install GH CLI

Follow [these instructions](https://github.com/cli/cli/blob/trunk/docs/install_linux.md).

### Login to DodekaComCom on GitHub

Using its password.

### Setup `gh`

Login using `gh auth login`, then use a correctly scoped auth token that you got from the DodekaComCom account.

### Add `backend` user to Docker group

```
sudo usermod -aG docker backend
```

Then logout and back in again.

### Login to ghcr.io

```
docker login ghcr.io
```

Use another access token, this one only has to read the org and have access to packages.


### Clone `dodeka`

```
gh repo clone dsav-dodeka/dodeka
```

### Set tidploy auth key

Now, ensure all necessary secrets are accessible by the access token you're going to set. Then enter the `dodeka` repository and do:

```
tidploy auth bws
```

Then enter the access token.

### Deploy

Next, run:

```
tidploy deploy -d deploy/use/production
```

This will start the backend and database.

### Optional: restore database

In case you have all the files from the volume that contained the database, you want to restore these. First, get them to the server, for example using `scp`. We assume we have a folder called `backup` in our current directory that contains all the Postgres files. Ensure the database is down again (using `docker compose -p dodeka-production-latest down`).

Then you can do:

```
docker run --rm -it -v ./backup:/from -v dodeka-db-volume-production-latest:/to alpine ash
```

This will put you into a container with the recently created, empty database at `/to` and the backup at `/from`. First, clean out the new folder using `rm -rf *` while inside the `/to` folder (don't do this in the container root directory!).

Then, you can run:

```
cd /from ; cp -av . /to
```

Now, restart the database. Everything should work now.

### Setup nginx

Install it:

```
sudo apt install nginx
```

Start it:

```
sudo systemctl start nginx
```

For some  extra details also see [the full section on nginx and certificates](./nginx_ssl.md). 

#### Setup non-HTTPS config

Go to `/etc/nginx`. Every file here is root-protected, so use `sudo` before each of the following commands:

Go to the `sites-available` subdirectory.

Do `nano api.dsavdodeka.nl` and paste the following basic config:

```nginx
server {
    root /var/www/api.dsavdodeka.nl/html;

    index index.html index.htm index.nginx-debian.html;

    server_name api.dsavdodeka.nl;

    location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            proxy_pass http://localhost:4241;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
    }

}

server {
    listen 80;
}
```

Create a symlink from the available to enabled:

```
sudo ln -s /etc/nginx/sites-available/api.dsavdodeka.nl /etc/nginx/sites-enabled/api.dsavdodeka.nl
```

If necessary, restart nginx:

```
sudo systemctl restart nginx
```

If you go to http://api.dsavdodeka.nl (not http**s**) you should get "Hallo: Atleten"!

### Certbot/Let's Encrypt

#### Install snap

```
sudo apt install snapd
```

#### Install certbot

```
sudo snap install --classic certbot
```

#### Run certbot

```
sudo certbot --nginx
```

First, enter your e-mail. Then it will give you a list of domains you want to install the certificate for, choose the number indicating `api.dsavdodeka.nl` (probably 1).


### Cleanup nginx config

You probably want to clean up your nginx config.

There might be a server block saying only:

```
server {
    listen 80;
}
```

Delete this, the Certbot block should handle this now.

If necessary, restart nginx:

```
sudo systemctl restart nginx
```

### Optional: install Python and Poetry

You can create database backups and migrate the database using Python. First, we need to install a Python version that has the same major version as the backend server requires.

To make it more easy to install new versions in the future, let's use [`pyenv`](https://github.com/pyenv/pyenv?tab=readme-ov-file#unixmacos). I recommend not installing using homebrew, as that might interfere with some other core packages. Instead, use their install script and follow the instructions to put it into the path. These were, when last checked:

```bash
curl https://pyenv.run | bash
```

To add it to path and load it automatically: add to `.bashrc` (in the server home folder):

```
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

Then, install the correct Python version using:

`pyenv install <exact version>`

Note that it will install Python from source, so this could take a while. If there is an error, take a look at all required [packages that must be installed](https://github.com/pyenv/pyenv/wiki#suggested-build-environment).

Then, go into the project directory and run `pyenv local <exact version>`. Now, the Python version should be the correct one if you run `python`. 

Next, we will install Poetry to manage our dependencies. I recommend using `pipx` (which you can just install using `sudo apt install pipx`), so `pipx install poetry`. 

Then, we want to make our Poetry environment use the correct version. Most likely, the Python version was installed into: `~/.pyenv/versions/<version>/bin/python`, so then you can use (once you are in the `backend/src` directory):

```bash
poetry env use ~/.pyenv/versions/<version>/bin/python
```

Now, we can run commands in our envrionment using `poetry run <command>`.

### Fin

That was it, with less than 300 lines of instructions can completely set up a Linux server from scratch, in a simple and secure way.