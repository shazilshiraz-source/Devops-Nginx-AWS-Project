# Phase 3 — Docker Setup (Dockerized Nginx on EC2)

## Overview

In this phase we deploy a simple web application using Docker on our EC2 instance.

Instead of installing Nginx directly on the server, we package Nginx inside a Docker container. This approach ensures the application runs in an isolated and reproducible environment.

The goal of this phase is to:

- Install Docker on the EC2 instance
- Create a Docker image using the nginx:alpine base image
- Add a custom HTML page
- Run the container on port 80
- Verify the application through the EC2 public IP

---

# What is Docker 

Docker is a containerization platform that allows applications to run inside **containers**.

A container includes:
- the application
- its dependencies
- runtime environment

This ensures the application runs the same way on any machine.

Instead of installing software directly on a server, we package it inside a container.

In this project we are running **Nginx inside a Docker container**.

---

# What is nginx:alpine

`nginx:alpine` is a lightweight Nginx Docker image based on Alpine Linux.

Benefits:
- very small image size
- fast startup
- minimal resources

This makes it ideal for cloud deployments.

---

# Step 1 — Connect to the EC2 Instance

Connect to the instance using SSH.

Example:

```bash
ssh -i Nginx-Key.pem ec2-user@<EC2_PUBLIC_IP>
```

Example:

```bash
ssh -i Nginx-Key.pem ec2-user@3.137.173.112
```

If connected successfully the terminal will show:

```
[ec2-user@ip-10-0-1-126 ~]$
```

---

# Step 2 — Install Docker

Update the system packages.

```bash
sudo yum update -y
```

Install Docker.

```bash
sudo yum install docker -y
```

Start the Docker service.

```bash
sudo systemctl start docker
```

Enable Docker to start automatically on boot.

```bash
sudo systemctl enable docker
```

Add the current user to the docker group.

```bash
sudo usermod -aG docker ec2-user
```

Log out and reconnect to apply group changes.

---

# Step 3 — Verify Docker Installation

Run the test container.

```bash
docker run hello-world
```

Expected output:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

This confirms Docker is installed and working correctly.

---

# Step 4 — Create Project Directory

Create a new directory for the Docker project.

```bash
mkdir nginx-project
cd nginx-project
```

---

# Step 5 — Create Custom index.html

Create an HTML file that will be served by Nginx.

```bash
nano index.html
```

Example content:

```html
<html>
<head>
<title>My Docker App</title>
</head>
<body>
<h1>Hello from Dockerized Nginx</h1>
<p>Deployed on AWS EC2 using Docker</p>
</body>
</html>
```

Save and exit.

---

# Step 6 — Create Dockerfile

Create the Dockerfile.

```bash
nano Dockerfile
```

Add the following content:

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```

Explanation:

| Instruction | Meaning |
|-------------|--------|
| FROM nginx:alpine | Uses Nginx as the base image |
| COPY | Copies our custom HTML file into the Nginx web directory |

---

# Step 7 — Build the Docker Image

Build the Docker image from the Dockerfile.

```bash
docker build -t my-nginx .
```

Explanation:

| Part | Meaning |
|-----|-----|
| docker build | Creates a Docker image |
| -t my-nginx | Tags the image with a name |
| . | Uses the current directory as the build context |

---

# Step 8 — Run the Docker Container

Start the container and map port 80.

```bash
docker run -d -p 80:80 my-nginx
```

Explanation:

| Part | Meaning |
|-----|-----|
| -d | Run container in background |
| -p 80:80 | Maps server port 80 to container port 80 |
| my-nginx | Image used to create container |

---

# Step 9 — Verify Running Containers

Check running containers.

```bash
docker ps
```

Example output:

```
CONTAINER ID   IMAGE      PORTS
a853b4c106ab   my-nginx   0.0.0.0:80->80/tcp
```

This confirms the container is running.

---

# Step 10 — Verify the Application

Open a browser and visit the EC2 public IP.

Example:

```
http://3.137.173.112
```

The browser should display the custom HTML page created earlier.

---

# Outcome

After completing this phase we successfully:

- Installed Docker on the EC2 instance
- Created a Docker image using nginx:alpine
- Built a custom container with our HTML page
- Ran the container on port 80
- Verified the application through the EC2 public IP

The application is now running inside a Docker container on AWS.

*Date: April 6, 2026* 