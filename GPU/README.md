# Distrobox with General Deep Learning Image on Ubuntu (with Podman and NVIDIA Toolkit)

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Setting up Podman and NVIDIA Toolkit](#setting-up-podman-and-nvidia-toolkit)
- [Setting up Distrobox with Deep Learning Image](#setting-up-distrobox-with-deep-learning-image)
- [Using TensorFlow or PyTorch inside Distrobox](#using-tensorflow-or-pytorch-inside-distrobox)
- [Managing Distrobox](#managing-distrobox)
- [Uninstalling Distrobox](#uninstalling-distrobox)
- [Troubleshooting](#troubleshooting)

---

## Introduction

This guide demonstrates how to set up **Distrobox** with a general-purpose deep learning container image, capable of running both **TensorFlow** and **PyTorch**, on **Ubuntu**. We will use **Podman** as the container engine and the **NVIDIA Container Toolkit** for GPU acceleration. This setup isolates your deep learning environment from the host system, allowing you to perform GPU-accelerated training and inference without impacting the host.

---

## Prerequisites

Before proceeding, make sure you have:
- [**Ubuntu**](https://releases.ubuntu.com/) 20.04+
- [**Podman**](https://podman.io/docs/installation) installed
- [**NVIDIA GPU**](https://www.nvidia.com/en-us/drivers/) with drivers installed
- [**NVIDIA Container Toolkit**](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation) set up for GPU support
- Basic understanding of containers, [PyTorch](https://pytorch.org/) and [TensorFlow](https://www.tensorflow.org/)


---

## Installation

### Step 1: Install Podman

You can follow these steps to install **Podman** on Ubuntu, or refer to the official [Podman installation documentation](https://podman.io/docs/installation) for more details.

Podman is a rootless container engine, which simplifies usage by eliminating the need for `sudo` access.

1. Update your package list and install Podman:
    ```bash
    sudo apt-get update
    sudo apt-get -y install podman
    ```

2. Verify the installation by checking the version:
    ```bash
    podman --version
    ```

### Step 2: Install Distrobox

Once **Podman** is installed, you can proceed to install [**Distrobox**](https://wiki.archlinux.org/title/Distrobox#Installation), which provides a lightweight containerized environment:

```bash
sudo apt install distrobox
```

After completing this step, you’ll be ready to manage your containers with Distrobox.

---

## Setting up Podman and NVIDIA Toolkit

### Step 1: Install NVIDIA Drivers and CUDA

Ensure your **NVIDIA drivers** and **CUDA** are installed on your system to enable GPU acceleration by checking [Nvidia Drivers](https://www.nvidia.com/en-us/drivers/) offical website. You can adjust the version according to your specific GPU requirements.

### Step 2: Install NVIDIA Container Toolkit

The [**NVIDIA Container Toolkit**](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation) is required to allow containers to access the GPU. Follow these steps to install it:

1. Add the NVIDIA repository to your system:
    ```bash
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

2. Update your package list and install the NVIDIA Container Toolkit:
    ```bash
    sudo apt-get update
    sudo apt-get install -y nvidia-container-toolkit
    ```

3. Configure the NVIDIA runtime for Podman like in this [link](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/cdi-support.html):
    ```bash
    sudo nvidia-ctk cdi generate --output=/etc/cdi/nvidia.yaml
    ```

4. Finally, restart the Podman service to apply the changes:
    ```bash
    sudo systemctl restart podman
    ```

After completing this section, your system should be set up to support GPU-accelerated containers using Podman and the NVIDIA toolkit.

---

## Setting up Distrobox with Deep Learning Image

### Step 1: Pull a General Deep Learning Container Image

To get started with a deep learning environment inside Distrobox, you need to pull an image that supports **PyTorch** for example. Use the following command to pull an image:

```bash
podman pull docker.io/pytorch/pytorch:2.4.1-cuda11.8-cudnn9-devel
```

This image includes **PyTorch** pre-installed, enabling you to work with that framework.

### Step 2: Create a Distrobox Container

Next, create a **Distrobox** container using the image you pulled:

```bash
distrobox-create --name dl-container --image docker.io/pytorch/pytorch:2.4.1-cuda11.8-cudnn9-devel --nvidia
```

This command creates a Distrobox environment based on the specified image and ensures that you have a fully isolated deep learning environment with GPU support.

### Step 3: Enter the Distrobox Container

Once the container is created, you can enter it and start using it:

```bash
distrobox-enter dl-container
```

Now, you are inside the container and ready to run deep learning tasks.

---

## Using PyTorch inside Distrobox

After entering the Distrobox container, you can verify if **PyTorch** is correctly set up and is utilizing GPU acceleration.

### Step 1: Verify GPU Support

1. Check if GPU is available:
    ```bash
    nvidia-smi -L
    ```

### Step 2: Verify PyTorch GPU Support

1. Start a Python session:
    ```bash
    python
    ```

2. Check if **PyTorch** detects the GPU:
    ```python
    import torch
    print(torch.cuda.is_available())  # This should return True if GPU is available
    ```

**PyTorch** should now be able to use your GPU for accelerated computations.

---

## Managing Distrobox

Here are some essential commands for managing your Distrobox containers:

### List All Distrobox Containers

To list all active Distrobox containers, run:
```bash
distrobox-list
```

### Enter an Existing Distrobox Container

You can enter a specific Distrobox container by using:
```bash
distrobox-enter dl-container
```

### Remove a Distrobox Container

If you no longer need a Distrobox container, remove it with:
```bash
distrobox-rm dl-container
```

---

## Uninstalling Distrobox

To remove **Distrobox** from your system, use the following command:
```bash
sudo apt remove distrobox
```

Additionally, you can uninstall **Podman** and the **NVIDIA Toolkit** if you no longer require them:
```bash
sudo apt remove podman nvidia-container-toolkit
```

---

## Troubleshooting

### GPU Not Detected

- Ensure that the **NVIDIA Container Toolkit** is installed and configured correctly.
- Run `podman info | grep nvidia` to verify if the NVIDIA runtime is available.
- Make sure your **NVIDIA drivers** and **CUDA** are properly installed.
- Some older images are not supported by distrobox, make sure to use recent container images
