# Reverse Proxy with Docker Compose

## Overview

This project demonstrates a simple Docker Compose setup where an Nginx container acts as a reverse proxy for a Python Flask backend.

The backend exposes an HTTP endpoint that returns:

```text
Hello from Backend
```

Nginx listens on port **8080** and forwards all incoming requests to the backend container.

## Network Configuration

Docker Compose creates a custom bridge network named `app-network`.

Both containers (`backend` and `nginx`) are attached to this network, allowing Nginx to communicate with the backend using the Docker service name:

```
http://backend:5000
```

The backend port is exposed only within the Docker network using:

```yaml
expose:
  - "5000"
```

The Nginx container publishes port **80** on host port **8080**:

```yaml
ports:
  - "8080:80"
```

## Running the Project

Build the backend image:

```bash
docker compose build
```

Start the containers:

```bash
docker compose up -d
```

Verify that the containers are running:

```bash
docker compose ps
```

Test the application:

```bash
curl http://localhost:8080
```

Expected output:

```text
Hello from Backend
```

Stop the containers:

```bash
docker compose down
```

## Deployment

This project was developed and tested on an AWS EC2 Ubuntu instance. The backend and Nginx containers were built and managed using Docker Compose. Nginx acts as a reverse proxy, forwarding incoming HTTP requests to the Flask backend over a custom Docker bridge network.

To access the application from outside the EC2 instance, ensure that the EC2 security group allows inbound traffic on the port exposed by the Nginx container (for example, port **8080** if using the mapping `8080:80`).
