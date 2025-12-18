# Hetzner

## Preparing SSH

First we rebuild the image from Ubuntu 24.04.

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

Paste in your SSH **public** key (generate one using `ssh-keygen -t ed25519 -C "your_email@example.com"`) (the public key looks something like `ssh-ed25519 .... tiptenbrink@tipc`) and save the file (Ctrl-X). If copy-paste is not working, maybe try a different browser and make sure you're not in GUI mode or similar.

Then, ensure the file has the correct permissions:

```
chmod 700 /home/backend/.ssh && chmod 600 /home/backend/.ssh/authorized_keys
```

Then you can log in with `ssh backend@<ip address here>` (make sure it uses the proper private key, so you might have to use the `-i` option). 

Note that it's often had to find out what's going on when it's not working. Be sure that the string in `authorized_keys` precisely matches your public key. Note that sometimes copy-pasting can not work correctly and some characters are changed (like = to -, or _ to -) or maybe you missed one character at the beginning or end. It really needs to match!

### SSH niceties

Install "xauth" (while logged in as root)

```
apt install xauth
```

If you're using a nice terminal emulator, you might have to add some xterm files. Consult the documentation of your terminal for details.

**From this point on we never need to be logged in as root anymore! Always log in via ssh from your terminal**

## Dependencies

### Update packages

```
sudo apt update
sudo apt upgrade
```

You might have to reboot after this: `reboot`.

### Install basic C compiler and other useful packages

```
sudo apt install unzip build-essential
```

### Install NodeJS

Using fnm:

```
curl -o- https://fnm.vercel.app/install | bash
```

Be sure to re-login/start new terminal.

Set-up NodeJS 24:

```
fnm use 24
```

Verify with `node -v`, should return something starting with `v24`.

### Install Python (with uv)

First, we install `uv`:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Then install Python (free-threading build) with:

```
uv python install 3.14t
```

### Install Go

Go here to get the latest version (for linux and amd64/x64):

```
https://go.dev/doc/install
```

Currently that would be go1.25.5.linux-amd64. Then download it like:

```
curl -OL https://go.dev/dl/go1.25.5.linux-amd64.tar.gz
```

Then put it in the install location with:

```
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.25.5.linux-amd64.tar.gz
```

Finally, add go to your path by appending the following to the end of `~/.profile`:

```
export PATH="$PATH:/usr/local/go/bin"
```

## Install backend (and frontend)

We did not strictly need to install NodeJS and the frontend as we will not host the frontend ourselves. However, I will explain how to here, since that can be useful as a demo.

### Frontend

First, let's clone the frontend to the home directory.

```
git clone https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io.git frontend --filter=blob:none
```

## Set up reverse proxy

### Installing Caddy

We will install Caddy. Go to [their docs](https://caddyserver.com/docs/install#debian-ubuntu-raspbian) for the precise command (the one with stable and containing sudo apt update among other things).

### `Caddyfile`

### Permissions

The Caddy server runs as the `caddy` user and if you want to run the demo using a file server, you will have to give it permission to access the files you generate when building the frontend. Therefore, we will create a new group and add both the backend and caddy users to it and give it access to the frontend build output.

The following snippets shows all commands you have to run.

```bash
# Create a shared group called "webdata"
sudo groupadd webdata
# groupadd: creates a new group
# webdata: the name of the group to create

# Add both users to the webdata group
sudo usermod -aG webdata backend
sudo usermod -aG webdata caddy
# usermod: modify a user account
# -a: append (add to group without removing from other groups)
# -G: specify supplementary group(s) to add the user to
# webdata: the group to add them to
# backend/caddy: the username to modify

# Change ownership of the client folder
sudo chown -R backend:webdata /home/backend/frontend/build/client
# chown: change file owner and group
# -R: recursive (apply to all files and subdirectories)
# backend:webdata: set owner to "backend" and group to "webdata"
# /home/...: the target directory

# Set permissions: owner full, group read+execute
sudo chmod -R 750 /home/backend/frontend/build/client
# chmod: change file mode/permissions
# -R: recursive
# 750: octal permission code
#   7 (owner): read(4) + write(2) + execute(1) = full access
#   5 (group): read(4) + execute(1) = can read and traverse
#   0 (others): no access

# Make new files inherit the group (setgid bit)
sudo chmod g+s /home/backend/frontend/build/client
# g+s: set the setgid bit on the directory
# This means new files/folders created inside will inherit
# the "webdata" group instead of the creator's primary group

# Ensure parent directories are traversable
chmod 755 /home/backend
chmod 755 /home/backend/frontend
chmod 755 /home/backend/frontend/build
# 755: 
#   7 (owner): full access
#   5 (group): read + execute
#   5 (others): read + execute
# "execute" on a directory means permission to traverse/enter it

# Restart caddy to pick up the new group membership
sudo systemctl restart caddy
# systemctl: control the systemd service manager
# restart: stop and start the service
# caddy: the service name

# Log out and back in (or run `newgrp webdata`) for backend user to pick up the new group
# newgrp webdata: starts a new shell with webdata as the active group
```
