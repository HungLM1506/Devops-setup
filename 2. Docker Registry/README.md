# Docker Registry Setup

This guide will help you set up a Docker Registry using a Docker Compose file.

## Prerequisites

- Docker installed on your machine
- Docker Compose installed on your machine

## Steps

1. Create a directory for your Docker Registry setup:

    ```sh
    mkdir docker-registry
    cd docker-registry
    mkdir data
    nano docker-compose.yml
    ```

2. Create a `docker-compose.yml` file with the following content:

    ```yaml
    services:
      registry:
        image: registry:2
        ports:
        - "5000:5000"
        environment:
        REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
        volumes:
        - ./data:/data
    ```

3. Start the Docker Registry:

    ```sh
    docker-compose up -d
    ```

4. Verify that the Docker Registry is running:

    ```sh
    curl http://<ip-server>:5000/v2/
    ```

    You should see an empty JSON object `{}` as a response.
5. Use another computer to log in:
- **Case 1**: Registry without HTTPS (for internal networks or testing purposes)
On the machine that wants to log in, modify the Docker daemon configuration to allow access to an insecure registry:
Open the Docker daemon configuration file:
    ```sh
    sudo nano /etc/docker/daemon.json
    ```
    Add the following configuration (if the file doesn’t exist, create it):
    ```json
    {
    "insecure-registries": ["<registry-ip>:5000"]
    }
    ```
    Restart Docker to apply changes:
    ```sh
    sudo systemctl restart docker
    ```
- **Case2**:Registry with HTTPS
If the Docker Registry is co'nfigured with HTTPS (using valid SSL certificates), you don’t need to configure `insecure-registries`.
On the machine that wants to log in, simply run:
    ```sh
    docker login <registry-ip>:5000
    ```

## Pushing an Image to Your Registry
1. Login 
    ```sh
    docker login <ip-server>:<port>
    ```
2. Tag an image to push to your registry:

    ```sh
    docker tag your-image:tag <ip-server>:5000/your-image:tag
    ```

3. Push the image to your registry:

    ```sh
    docker push <ip-server>:5000/your-image:tag
    ```

4. Verify that the image is in your registry:

    ```sh
    curl http://<ip-server>:5000/v2/_catalog
    ```

    You should see a list of repositories in your registry.

## Pulling an Image from Your Registry

1. Pull the image from your registry:

    ```sh
    docker pull <ip-server>:5000/your-image:tag
    ```

## Stopping the Docker Registry

1. Stop the Docker Registry:

    ```sh
    docker compose down
    ```

## Conclusion

You have successfully set up a Docker Registry using Docker Compose. You can now push and pull images to and from your local registry.
