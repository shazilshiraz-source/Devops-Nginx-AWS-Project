# Phase 4 — Route 53 DNS Configuration

## Objective
Configure DNS so that the application running on the EC2 instance can be accessed using a **domain name** instead of a public IP address.

---

## Services Used

- AWS EC2
- AWS Route 53
- Elastic IP

---

## Architecture

User Browser
│
▼
Domain Name (dockerized-nginx-app.iac.ebricks-inc.com)
│
▼
Route53 DNS
│
▼
Elastic IP
│
▼
EC2 Instance
│
▼
Dockerized Nginx Application

---

## Step 1 — Allocate an Elastic IP

Navigate to:

AWS Console → EC2 → Elastic IPs

### Actions

1. Click **Allocate Elastic IP**
2. Associate the Elastic IP with the running EC2 instance
3. Note the Elastic IP address

Example:

Elastic IP: 18.190.198.225

Elastic IP ensures the public IP address of the instance **remains static even after instance restart**.

---

## Step 2 — Create Hosted Zone in Route 53

Navigate to:

AWS Console → Route 53 → Hosted Zones

### Steps

1. Click **Create Hosted Zone**
2. Enter the domain name

Example:

iac.ebricks-inc.com

3. Select:

Type: Public Hosted Zone

---

## Step 3 — Create DNS Record

Inside the hosted zone click:

Create Record

Configure the DNS record as follows:

| Field | Value |
|------|------|
| Record Name | dockerized-nginx-app |
| Record Type | A |
| Value | Elastic IP |
| TTL | 300 seconds |
| Routing Policy | Simple |

Example DNS record:

dockerized-nginx-app.iac.ebricks-inc.com

Points to:

18.190.198.225

---

## Step 4 — Verify DNS Configuration

Open a browser and access:

http://dockerized-nginx-app.iac.ebricks-inc.com

Expected result:

The **Nginx application running inside Docker on the EC2 instance should load successfully**.

---

## TTL Explanation

TTL (Time To Live) determines how long DNS resolvers cache a DNS record before requesting it again.

Example:

TTL = 300 seconds

This means DNS servers cache the record for **5 minutes**.

Typical TTL values:

| TTL | Meaning |
|----|----|
| 60 | 1 minute |
| 300 | 5 minutes |
| 3600 | 1 hour |

Lower TTL values help during testing because DNS changes propagate faster.

---

## DNS Record Types Used

### A Record

An **A Record** maps a domain name to an IPv4 address.

Example:

dockerized-nginx-app.iac.ebricks-inc.com → 18.190.198.225

---

## Alias Records (AWS Specific)

Alias records allow Route 53 to point domains directly to AWS resources such as:

- Load Balancers
- CloudFront
- S3 buckets

Alias records are **not required when pointing directly to an Elastic IP**.

---

## Troubleshooting

### DNS_PROBE_FINISHED_NXDOMAIN

This error occurs when DNS cannot resolve the domain.

Possible causes:

- DNS record not created
- Incorrect record name
- DNS propagation delay
- Typo in the domain name

---

### SSH Connection Timeout

If SSH fails:

ssh: connect to host <IP> port 22: Operation timed out

Check:

EC2 → Security Groups → Inbound Rules

Ensure the following rule exists:

| Type | Port | Source |
|-----|-----|-----|
| SSH | 22 | My IP |

---

## Outcome

After completing this phase:

- The Dockerized Nginx application is accessible using a **domain name instead of a public IP**.
- DNS resolution is handled by **AWS Route 53**.

*Date: April 10, 2026*