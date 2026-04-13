# Dockerized Nginx Application on AWS EC2

## Project Overview

This project demonstrates how to deploy a simple static website inside a Docker container using Nginx and host it on an AWS EC2 instance.

The application is containerized using Docker and served through an Nginx web server. The EC2 instance is accessible through an Elastic IP and a custom domain configured with Route53.


## Technologies Used

- AWS VPC
- AWS EC2
- AWS Route53
- Docker
- Nginx
- Linux (Amazon Linux)
- GitHub

---

## Project Files

```
Dockerfile
index.html
README.md
```

---

## Dockerfile

```
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

---

## Setup Steps

### 1. Launch EC2 Instance

- Create an EC2 instance using Amazon Linux
- Configure security group rules to allow:
  - SSH (Port 22)
  - HTTP (Port 80)

---

### 2. Connect to EC2

```
ssh -i your-key.pem ec2-user@<public-ip>
```

---

### 3. Install Docker

```
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
```

---

### 4. Build Docker Image

```
docker build -t my-nginx .
```

---

### 5. Run Docker Container

```
docker run -d -p 80:80 my-nginx
```

---

### 6. Access the Website

Open your browser and visit:

```
http://<18.190.198.225>
```

or

```
http://dockerized-nginx-app.iac.ebricks-inc.com
```

---

## Result

The website runs inside a Docker container using Nginx and is hosted on an AWS EC2 instance.  
The application can be accessed through the Elastic IP or a Route53 DNS record.

---

## Author

Shazil Shiraz  
DevOps Learning Project
