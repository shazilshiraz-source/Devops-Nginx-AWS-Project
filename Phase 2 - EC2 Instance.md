# Phase 2 — EC2 Instance Setup & SSH Access

## 1. Launch EC2 Instance

### Steps
1. Go to AWS Console
2. Open **EC2 Dashboard**
3. Click **Launch Instance**

### Instance Configuration

| Setting | Value |
|------|------|
| Name | devops-nginx-server |
| AMI | Amazon Linux 2023 |
| Instance Type | t2.micro (Free tier eligible) |
| Key Pair | Create new key pair (`Nginx-Key.pem`) |
| Network | Select the VPC created in Phase 1 |
| Subnet | **Public Subnet** |
| Auto Assign Public IP | Enabled |

Download the key pair file.

---

# 2. Fix Private Key Permissions



```bash
cd ~/Downloads
```

---

## 3. Change the Permission of the Key File

```bash
chmod 400 Nginx-Key.pem
```

### Why This Is Required

SSH refuses to use keys that are publicly accessible.

This command makes the file **read-only for the owner**, which satisfies SSH security requirements.

---

## 4. Verify Instance Public IP

After launching the instance:

1. Go to **EC2 → Instances**
2. Select the instance
3. Copy the **Public IPv4 address**

Example:

```
3.137.173.112
```

---

## 5. Configure Security Group

Edit the **Inbound Rules** of the instance security group.

### Rules

| Type  | Protocol | Port | Source |
|------|------|------|------|
| SSH | TCP | 22 | Your Public IP |
| HTTP | TCP | 80 | 0.0.0.0/0 |
| HTTPS | TCP | 443 | 0.0.0.0/0 |

Example personal IP:

```
119.73.101.32/32
```

---

## 6. Fix EC2 Instance Connect Access

The **AWS Console Connect button** uses AWS managed IP ranges.

To allow console connection we added:

```
3.16.146.0/29
```

### Final SSH Rules

| Type | Port | Source |
|------|------|------|
| SSH | 22 | 119.73.101.32/32 |
| SSH | 22 | 3.16.146.0/29 |

---

## 7. Connect to the Instance Using SSH

Run from the directory containing the `.pem` file.

```bash
ssh -i Nginx-Key.pem ec2-user@3.137.173.112
```

### Explanation

| Part | Meaning |
|------|------|
| ssh | Secure connection |
| -i Nginx-Key.pem | Authentication key |
| ec2-user | Default user for Amazon Linux |
| 3.137.173.112 | EC2 Public IP |

---

## 8. First Connection Confirmation

SSH shows a fingerprint warning the first time.

```
The authenticity of host can't be established.
Are you sure you want to continue connecting?
```

Type:

```
yes
```

This adds the server to the **known hosts list**.

---

## 9. Successful Login

After connecting you should see a prompt like:

```
[ec2-user@ip-10-0-1-23 ~]$
```

This confirms:

- Instance is reachable
- SSH configuration works
- Security group is correct
- Public subnet routing works

---

# Outcome

Successfully completed **Phase 2**:

- Launched EC2 instance
- Attached to public subnet
- Created key pair
- Configured security group
- Connected using SSH



*Date: April 3, 2026*