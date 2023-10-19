# Server

Our server is hosted by Hetzner, a German cloud provider. Our server is an unmanaged Linux Ubuntu virtual machine (VM). VM means that we do not have a full system to ourselves, but share it with other Hetzner customers. We have access to a limited number of cores, memory and storage.

It is unmanaged because we have full control over the operating system. We need to keep it up to date ourselves. The choice for Ubuntu was also made by us. It has no GUI, only a command line, so getting familiar with the Unix command line is very helpful. By default, it uses the `bash` shell.

## Webportal

The webportal for the server can be accessed from <https://console.hetzner.cloud>. The account we use is studentenatletiek@av40.nl. You need 2FA to log in.

The most important things you can do from the portal are:

- See graphs of CPU, disk and network load, as well as memory usage
- Manage backups
- Access to root console

## Connecting: SSH

To connect to the server, we use SSH. To be able to connect, you need to have an "SSH key" configured. To add one, you must first generate a private-public SSH keypair.

Then, the public part must be added to the `~/backend/.ssh/authorized_keys` file. Note, this file requires some specific permissions, so if something is not working check whether these are correct.

Currently, we have the following important SSH settings:

```
PermitRootLogin no
PasswordAuthentication no
```

This ensure you can only log in with an SSH key, not with a simple user password.

We only allow logins through the `backend` user (see next section), so keep `/root/.ssh/authorized_keys` empty.

It is recommended to not add SSH keys through the web console, as these are not easily visible inside the authorized_keys file.

If you no longer have access to any keys, use the web console to log in as root, then change user to backend `su backend` and edit the authorized_keys file.

### Connecting

Connecting is simple, simply do `ssh backend@<ip address>`. If your default identity is not a key that has access, you might need to use the `-i` flag to select the right key on your client.

Once you have done this, you have access to the server as if it's your PC's own command line.

## `root` vs `backend` user

To keep things safe, try to avoid using the `root` user as much as possible. Instead, use `backend`. You can use `sudo` to run priviledged commands and and if necessary, log in as root using `su root`. 

## Keeping it up to date

To keep the server up to date, ocassionally run:

```
sudo apt update
sudo apt install
```

Ocasionally, Ubuntu itself might also get an update. It is best to only update once there is a new LTS version. 

## Required packages

For running deployment scripts, three main tools must be installed, poetry, Docker (including Docker Compose) and the GitHub CLI. Make sure these are occasionally updated.

Furthermore, the server also requires nginx as a reverse proxy and certbot for SSL certificates. We use Ubuntu's packaged nginx and we use a snap package for certbot.

## File locations

Currently, mailcow is in /opt/mailcow-dockerized and the dodeka repository is in /home/backend/dodeka.