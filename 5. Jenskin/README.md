# Jenkins Setup with Docker

This guide will help you set up Jenkins using Docker.

## Prerequisites

- Docker installed on your machine
- Docker Compose installed

## Steps

1. **Create a Docker Compose file**

    Create a `docker-compose.yml` file with the following content:

    ```yaml
    services:
      jenkins:
         image: jenkins/jenkins:lts
         container_name: jenkins
         ports:
            - "8080:8080"
            - "50000:50000"
         volumes:
            - jenkins_home:/var/jenkins_home

    volumes:
      jenkins_home:
    ```

2. **Start Jenkins**

    Run the following command to start Jenkins:

    ```sh
    docker compose up -d
    ```

3. **Access Jenkins**

    Open your browser and go to `http://localhost:8080`. You should see the Jenkins setup screen.

4. **Get the Initial Admin Password**

    Run the following command to get the initial admin password:

    ```sh
    docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
    ```

    Copy the password and paste it into the Jenkins setup screen.

5. **Follow the Setup Wizard**

    Follow the on-screen instructions to complete the Jenkins setup.

## Stopping Jenkins

To stop Jenkins, run:

```sh
docker compose down
```

## Additional Configuration

You can customize the `docker-compose.yml` file to suit your needs, such as adding more plugins or configuring environment variables.
