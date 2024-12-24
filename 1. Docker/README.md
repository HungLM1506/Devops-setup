# Docker Setup Guide

This guide will help you install Docker on your system.

## Prerequisites

- A 64-bit operating system
- Administrative privileges

## Installation Steps

### Windows

1. Download Docker Desktop from the [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows).
2. Run the installer and follow the instructions.
3. Once the installation is complete, Docker will start automatically.

### macOS

1. Download Docker Desktop from the [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac).
2. Open the downloaded `.dmg` file and drag Docker to the Applications folder.
3. Open Docker from the Applications folder.

### Linux

#### Ubuntu

1. Update your existing list of packages:
    ```sh
    sudo apt update
    ```
2. Install Docker using the following commands:
    ```sh
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt update
    apt-cache policy docker-ce
    sudo apt install docker-ce
    sudo systemctl status docker
    ```

#### CentOS

1. Remove any old versions:
    ```sh
    sudo yum remove docker docker-common docker-selinux docker-engine
    ```
2. Install Docker using the following commands:
    ```sh
    sudo yum install -y yum-utils device-mapper-persistent-data lvm2
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install docker-ce
    sudo systemctl start docker
    ```

## Verify Installation

Run the following command to verify that Docker is installed correctly:
```sh
docker --version
```

You should see the Docker version information.

## Post-Installation Steps

### Manage Docker as a non-root user

1. Create the `docker` group:
    ```sh
    sudo groupadd docker
    ```
2. Add your user to the `docker` group:
    ```sh
    sudo usermod -aG docker $USER
    ```
3. Log out and log back in so that your group membership is re-evaluated.

### Enable Docker to start on boot

```sh
sudo systemctl enable docker
```

## Uninstallation

To uninstall Docker, follow the instructions for your operating system from the [Docker documentation](https://docs.docker.com/get-docker/).

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
