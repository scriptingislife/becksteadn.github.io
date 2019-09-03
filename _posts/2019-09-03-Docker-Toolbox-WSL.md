---
published: true
layout: post
title: Connecting WSL to Docker Toolbox on Windows 10 Home
---

Here's how to run Docker on Windows 10 and connect it to the Windows Subsystem for Linux (WSL) CLI.

## Install Docker Toolbox

Docker for Windows requires at least the Pro edition. Toolbox is a "legacy desktop solution" that will run Docker on Windows 10 Home. It runs the Docker host in a Linux virtual machine but comes with tools like the Docker Quickstart Terminal that are preconfigured to work with this 'remote' machine.

Follow the steps in the [Docker documentation](https://docs.docker.com/toolbox/toolbox_install_windows/) to install Docker Toolbox.

## Install Windows Subsystem for Linux (WSL)

Follow the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to install WSL. Ubuntu is the most popular choice.

## Install and Configure Docker Client in WSL

Install the Docker CLI.

`sudo apt install docker.io`

The virtual machine running Docker is managed by the `docker-machine` command. Get the URL of the remote Docker host by typing the following command. If the machine is stopped, run `docker-machine.exe start`.

`docker-machine.exe ls`

Set the DOCKER_HOST to that URL given from the above command. Add it to `~/.bash_profile` to start every time a console is launched.

Example URL: `tcp://192.168.99.100:2367`

`echo "export DOCKER_HOST=<url>" >> ~/.bash_profile`

Configure TLS for the Docker host.

`echo "export DOCKER_TLS_VERIFY=1" >> ~/.bash_profile`

`ln -s /mnt/c/Users/<username>/.docker/machine/certs /home/<username>/.docker`

Test it out.

`docker run hello-world`

## Resources

[Docker Toolbox Docs](https://docs.docker.com/toolbox/toolbox_install_windows/)

[WSL Docs](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
