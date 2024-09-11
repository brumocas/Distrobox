# Distrobox Crash Course

Welcome to the **Distrobox Crash Course**! This guide will help you get started with Distrobox, a tool that allows you to manage containers within your Linux environment. With Distrobox, you can run different Linux distributions and packages inside a container, all seamlessly integrated with your host system.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
  - [Create a Container](#create-a-container)
  - [Manage Containers](#manage-containers)
  - [Run Applications](#run-applications)
- [Tips & Tricks](#tips--tricks)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

## Introduction
Distrobox allows you to create and manage containers that run various Linux distributions within your main Linux environment. It integrates seamlessly, enabling you to use graphical applications, package managers, and terminal tools from different distributions as if they were native to your host system.

The crash course video on Distrobox can be found [here](https://www.youtube.com/watch?v=eiDt4O6UPRw).

## Features
- Run multiple Linux distributions in containers.
- Use graphical applications and CLI tools from containers on the host.
- Leverage the package management of different distributions.
- Seamlessly access files between containers and host systems.
- No need for elevated privileges (root access).

## Installation

### Prerequisites
Ensure you have the following:
- A Linux distribution.
- Podman or Docker installed.

### Install Distrobox

1. Install Distrobox using your system’s package manager, or use the curl script for a quick installation:
   ```bash
   curl -s https://raw.githubusercontent.com/89luca89/distrobox/main/install | sudo sh
   ```

2. Verify the installation by running:
   ```bash
   distrobox --help
   ```

## Usage

### Create a Container

1. To create a new container with a specific distribution (e.g., Ubuntu), run:
   ```bash
   distrobox-create --name my-ubuntu-box --image ubuntu:latest
   ```

2. Enter the container:
   ```bash
   distrobox-enter my-ubuntu-box
   ```

Once inside, you can install packages and run applications as if you were using a standalone Ubuntu system.

### Manage Containers

- List all available containers:
  ```bash
  distrobox-list
  ```

- Enter an existing container:
  ```bash
  distrobox-enter <container-name>
  ```

- Stop and remove a container:
  ```bash
  distrobox-rm <container-name>
  ```

### Run Applications

You can run both graphical and CLI applications from inside the Distrobox container. If you want to execute an app directly from your host system (without entering the container), use:
```bash
distrobox-enter --name my-ubuntu-box -- command-to-run
```

For example, to run `firefox` from a Fedora container:
```bash
distrobox-enter --name my-fedora-box -- firefox
```

## Tips & Tricks

- **Access host files**: Filesystem integration allows you to access your host files directly from within the container. No extra configuration is needed.
  
- **Use aliases**: Set up aliases in your shell for quick access to containers:
  ```bash
  alias enter-ubuntu="distrobox-enter my-ubuntu-box"
  ```

- **Persistent configurations**: Configuration changes made inside the container, such as installing packages or modifying settings, are retained even after exiting.

## Troubleshooting

- **Container not starting**: Ensure that Podman or Docker is running correctly. Use:
  ```bash
  systemctl status podman
  ```
  or
  ```bash
  systemctl status docker
  ```

- **Permission issues**: Make sure the user running Distrobox has sufficient permissions to use Podman or Docker without root privileges.

- **Slow application startup**: Running graphical applications may be slow at first, but performance should improve over time.

## Resources

- Official Distrobox GitHub: [Distrobox](https://github.com/89luca89/distrobox)
- Official Crash Course Video: [YouTube](https://www.youtube.com/watch?v=eiDt4O6UPRw)
- Podman Documentation: [Podman](https://podman.io/getting-started/)
- Docker Documentation: [Docker](https://docs.docker.com/)

Feel free to explore and experiment with different distributions within Distrobox. Happy hacking!
