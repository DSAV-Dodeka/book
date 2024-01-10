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