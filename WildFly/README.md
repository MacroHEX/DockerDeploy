# JBoss Wildfly Deployment Guide

This guide provides instructions on how to deploy applications using JBoss Wildfly in Docker containers.

## Choosing the Wildfly Version

Select the appropriate Wildfly version based on your requirements:

- For versions **25.0.0 final and later**:
    - Use the image from Quay.io. Find it [here](https://quay.io/repository/wildfly/wildfly?tab=info).
- For versions **before 25.0.0 final**:
    - Use the image from Docker Hub. Find it [here](https://hub.docker.com/r/jboss/wildfly).

## Dockerfile Configuration

Below is a template for the `Dockerfile`. Adjust the `FROM` line according to the chosen Wildfly version:

```dockerfile
# Use for versions < 25.0.0
# FROM jboss/wildfly

# Use for versions >= 25.0.0
# FROM quay.io/wildfly/wildfly

# Add your WAR file to the WildFly deployments directory
COPY ./path/to/your/app.war /opt/jboss/wildfly/standalone/deployments/

# Optional: Add a custom configuration file if needed
# COPY ./path/to/your/custom-config.xml /opt/jboss/wildfly/standalone/configuration/

# Expose the port the application will run on
EXPOSE 8080

# Set the default command to run on boot
# This will boot WildFly in the standalone mode and bind to all network interfaces
CMD ["/opt/jboss/wildfly/bin/standalone.sh", "-b", "0.0.0.0"]
```

## Building the image

Build your Docker image with the following command. Replace my-wildfly-app with your desired image name, and . with the
path to the directory containing your Dockerfile.

```bash
docker build -t my-wildfly-app .
```

## Running the docker container

Run the Docker container using the built image with this command. Replace my-wildfly-app with the name you used when
building the image.

```bash
docker run -p 8080:8080 my-wildfly-app
```

The -p 8080:8080 option maps port 8080 of the container to port 8080 on your host machine, allowing you to access the
application via http://localhost:8080.

## Running in Detached Mode

If you prefer to run your container in the background, use the -d flag:

```bash
docker run -d -p 8080:8080 my-wildfly-app
```

This will start the container in detached mode, freeing up your terminal while keeping the container running.

## Running with Docker Compose

To run the Docker image using Docker Compose, create a `docker-compose.yml` file in the same directory as your
Dockerfile. Here's a basic template to get you started:

```yaml
version: '3'
services:
  wildfly:
    image: my-wildfly-app
    ports:
      - "8080:8080"
    environment:
      - JAVA_OPTS=-Djava.awt.headless=true
```

To start the container using Docker Compose, run the following command:

```bash
docker-compose up
```

This will start the WildFly application server on port 8080. You can access the application at http://localhost:8080.
