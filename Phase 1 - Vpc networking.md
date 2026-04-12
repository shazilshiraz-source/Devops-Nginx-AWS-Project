# DevOps Internship Notes
## Project: Deploy a Dockerized Nginx App on AWS
### Phase 1 – VPC & Networking

Today I configured the networking infrastructure in AWS required to deploy a Dockerized Nginx application.

All networking resources were created inside **AWS VPC**.

---

# 1. What is a VPC?

A **VPC (Virtual Private Cloud)** is a private network inside AWS where cloud resources such as servers, databases, and containers are deployed.

It allows you to control:
- IP address ranges
- Subnets
- Routing
- Internet connectivity

In this project, I created a VPC with the following configuration:
VPC Name: vpc-01
CIDR Block: 10.0.0.0/16

This CIDR block defines the IP range available for resources inside the VPC.

---

# 2. What is CIDR?

CIDR stands for **Classless Inter-Domain Routing**.

It defines the **range of IP addresses available in a network**.

Example used in this project: 10.0.0.0/16
This means the VPC can generate addresses like:
10.0.0.1
10.0.1.10
10.0.50.25
CIDR blocks allow networks to be divided into smaller sections called **subnets**.

---

# 3. Subnets

A **Subnet** is a smaller network inside a VPC.

Subnets allow infrastructure to be separated based on accessibility and security.

Two types of subnets were created:

### Public Subnet
CIDR: 10.0.1.0/24

Resources inside the public subnet can communicate with the internet.

Typical services placed here include:

- Web servers
- Load balancers
- APIs

---

### Private Subnet

CIDR: 10.0.2.0/24
Resources inside the private subnet are **not accessible from the internet**.

Typical services placed here include:

- Databases
- Internal services
- Backend applications

---

# 4. Internet Gateway

An **Internet Gateway (IGW)** allows resources in a VPC to connect to the internet.

Steps performed:

1. Created an Internet Gateway
2. Attached it to the VPC

Without an Internet Gateway, the VPC would remain completely isolated.
10.0.0.0/16 → local
0.0.0.0/0 → Internet Gateway
Explanation:

- `10.0.0.0/16: local` allows internal communication within the VPC.
- `0.0.0.0/0:`git IGW` allows internet traffic to leave the VPC through the Internet Gateway.

This route table was associated with the **public subnet**.

---

## Private Route Table

The private subnet uses the default route table which contains only:


10.0.0.0/16: local


Since there is **no route to the Internet Gateway**, resources inside this subnet cannot access the internet directly.

---

# Outcome of Phase 1

Successfully configured:

- VPC
- Public Subnet
- Private Subnet
- Internet Gateway
- Public Route Table
- Subnet associations
                                                                                    


*Date: April 2, 2026*