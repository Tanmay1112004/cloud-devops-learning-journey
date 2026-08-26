# DevOps Day 04 — AWS EC2: Launch a Linux Server & Host a Website

## Learning Objectives

- Understand Cloud Computing and AWS
- Understand EC2, AMI and EC2 instances
- Understand Amazon Linux
- Understand SSH and key pairs
- Understand Security Groups and ports
- Launch an EC2 instance
- Connect to EC2 using SSH
- Install Python and Apache
- Host a website on EC2
- Troubleshoot a basic web server
- Understand Stop vs Terminate

## 1. Cloud Computing

Cloud computing means using computing resources over the internet instead of purchasing and maintaining physical infrastructure yourself.

```text
Company
   |
 Internet
   |
Cloud Provider
   |
   ├── Compute
   ├── Storage
   ├── Database
   └── Networking
```

## 2. AWS

**AWS = Amazon Web Services**

AWS is Amazon's cloud platform.

Common services:

```text
Compute       → EC2
Storage       → S3
Database      → RDS
Networking    → VPC
DNS           → Route 53
Serverless    → Lambda
Monitoring    → CloudWatch
Identity      → IAM
```

Official website: https://aws.amazon.com/

## 3. Major Cloud Providers

| Provider | Company |
|---|---|
| AWS | Amazon |
| Azure | Microsoft |
| GCP | Google |
| Alibaba Cloud | Alibaba |
| Oracle Cloud | Oracle |
| IBM Cloud | IBM |

## 4. What is EC2?

**EC2 = Elastic Compute Cloud**

EC2 provides virtual servers in AWS.

```text
Your Laptop
     |
  Internet
     |
     v
    AWS
     |
     v
    EC2
     |
     v
Virtual Server
```

## 5. EC2 Instance

An EC2 instance is a virtual server running in AWS.

It can have:

- Operating system
- Instance type
- Storage
- Security Group
- Key pair
- Public/private networking

## 6. AMI

**AMI = Amazon Machine Image**

An AMI is a template used to launch an EC2 instance.

```text
AMI
 ↓
OS + Configuration Template
 ↓
EC2 Instance
```

## 7. Amazon Linux

Amazon Linux is a Linux distribution provided by AWS.

Package management commonly uses:

```bash
dnf
```

`yum` may also be available as a compatibility interface depending on the Amazon Linux version.

```text
Ubuntu / Debian → apt
Amazon Linux    → dnf / yum
```

## 8. Launch EC2

Basic workflow:

```text
AWS Console
     ↓
EC2
     ↓
Launch Instance
     ↓
Name
     ↓
AMI
     ↓
Instance Type
     ↓
Key Pair
     ↓
Security Group
     ↓
Launch
```

### Steps

1. Login to AWS Console.
2. Search for **EC2**.
3. Name the instance, e.g. `server`.
4. Select **Amazon Linux**.
5. Select the required instance type, e.g. `t3.micro`.
6. Create/download a key pair such as `key.pem`.
7. Configure Security Group.
8. Allow required traffic.
9. Launch the instance.

> AWS pricing and free-usage eligibility can change. Always check the current AWS pricing/free-usage information before launching resources.

## 9. Key Pair

A key pair is used for secure authentication to an EC2 instance.

```text
Key Pair
   |
   ├── Public Key → EC2
   |
   └── Private Key → You
```

Protect the private key.

**Never upload `.pem` files to GitHub.**

Add to `.gitignore`:

```gitignore
*.pem
.env
```

## 10. SSH

**SSH = Secure Shell**

SSH is used to securely access a remote server.

Default port:

```text
22
```

Typical command:

```bash
chmod 400 key.pem
ssh -i key.pem ec2-user@PUBLIC_IP
```

For Amazon Linux, `ec2-user` is commonly the default username.

## 11. Security Group

A Security Group acts as a virtual firewall for an EC2 instance.

| Service | Port |
|---|---:|
| SSH | 22 |
| HTTP | 80 |
| HTTPS | 443 |

```text
Internet
   |
   v
Security Group
   |
   ├── 22  → SSH
   ├── 80  → HTTP
   └── 443 → HTTPS
   |
   v
EC2
```

For a lab, HTTP/HTTPS may be opened broadly. For real environments, restrict SSH access to trusted IP ranges whenever practical.

## 12. Check OS

After SSH:

```bash
cat /etc/os-release
```

Always identify the operating system before running administration commands.

## 13. Update Amazon Linux

```bash
sudo dnf update -y
```

You may also encounter:

```bash
sudo yum update -y
```

## 14. Install Python

```bash
sudo dnf install python -y
python --version
```

If necessary:

```bash
python3 --version
```

## 15. Install Apache

On Amazon Linux:

```bash
sudo dnf install httpd -y
```

Start:

```bash
sudo systemctl start httpd
```

Check:

```bash
sudo systemctl status httpd
```

Enable at boot:

```bash
sudo systemctl enable httpd
```

Or:

```bash
sudo systemctl enable --now httpd
```

### Correct commands

Incorrect:

```bash
sudo system status httpd
sudo system enable httpd
```

Correct:

```bash
sudo systemctl status httpd
sudo systemctl enable httpd
```

## 16. Apache Document Root

On Amazon Linux Apache commonly serves website files from:

```text
/var/www/html
```

```bash
cd /var/www/html
pwd
ls -la
```

## 17. Host a Website

Create:

```bash
sudo touch index.html
sudo nano index.html
```

Add:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AWS EC2 Website</title>
</head>
<body>
    <h1>Welcome to My AWS EC2 Web Server</h1>
    <p>This website is hosted on Amazon Linux using Apache.</p>
</body>
</html>
```

Check:

```bash
cat index.html
```

## 18. Test Locally

```bash
curl http://localhost/
```

## 19. Access from Browser

Use:

```text
http://PUBLIC-IP/
```

HTTP uses port 80 by default, so `:80` is normally unnecessary.

## 20. Complete Architecture

```text
                  INTERNET
                     |
                 Public IP
                     |
                     v
             +---------------+
             | Security Group|
             |               |
             | 22  → SSH     |
             | 80  → HTTP    |
             | 443 → HTTPS   |
             +-------+-------+
                     |
                     v
              +-------------+
              | EC2 Instance|
              | Amazon Linux|
              +------+------+
                     |
                     v
                  Apache
                  httpd
                     |
                     v
              /var/www/html
                     |
                     v
                index.html
                     |
                     v
                  Website
```

## 21. Troubleshooting

If the website does not open:

### Check EC2
Make sure the instance is running.

### Check Apache

```bash
sudo systemctl status httpd
```

### Check port 80

```bash
sudo ss -lntp | grep :80
```

### Test locally

```bash
curl http://localhost/
```

### Check website

```bash
ls -la /var/www/html
cat /var/www/html/index.html
```

### Check Security Group

Make sure inbound HTTP/TCP port 80 is allowed.

### Check Public IP

Make sure you are using the current public IP.

### Troubleshooting flow

```text
Website not loading
       |
       v
Is EC2 running?
       |
       v
Is Apache running?
       |
       v
Is Apache listening on port 80?
       |
       v
Does curl localhost work?
       |
       v
Is Security Group allowing port 80?
       |
       v
Is public IP correct?
       |
       v
Check firewall / networking / logs
```

## 22. Stop vs Terminate

### Stop

```text
Running → Stopped
```

You can generally start the instance again later. Some resources, such as attached storage, can still incur charges.

### Terminate

```text
Running → Terminated
```

The instance is deleted and termination is generally irreversible for that instance.

Use **Stop** when you want to keep the lab instance.

Use **Terminate** when you are completely finished with it.

Always review remaining resources and current AWS pricing to avoid unexpected charges.

## 23. Interview Questions

### Q1. What is AWS?

> AWS stands for Amazon Web Services. It is a cloud platform providing services such as compute, storage, databases, networking, security and monitoring.

### Q2. What is EC2?

> EC2 stands for Elastic Compute Cloud. It is an AWS service that provides scalable virtual servers called instances.

### Q3. What is an EC2 instance?

> An EC2 instance is a virtual server running in AWS. We can choose its operating system, instance type, storage, networking and security configuration.

### Q4. What is an AMI?

> AMI stands for Amazon Machine Image. It is a template used to launch EC2 instances.

### Q5. What is a Security Group?

> A Security Group is a virtual firewall associated with resources such as EC2 instances. It controls network traffic using configured rules.

### Q6. What port does SSH use?

> SSH commonly uses TCP port 22.

### Q7. What port does HTTP use?

> HTTP commonly uses TCP port 80.

### Q8. What port does HTTPS use?

> HTTPS commonly uses TCP port 443.

### Q9. Why do we need a key pair?

> A key pair provides secure authentication for connecting to an EC2 instance, commonly through SSH. The private key must be protected.

### Q10. What is Amazon Linux?

> Amazon Linux is a Linux distribution provided by AWS and optimized for AWS environments.

### Scenario: Website Not Working

**Interviewer:** You launched an EC2 instance and installed Apache, but the website is not accessible. What will you check?

**Strong answer:**

> First, I would check whether the EC2 instance is running. Then I would check Apache using `systemctl status httpd` and verify that it is listening on port 80. I would test locally using `curl http://localhost`. If it works locally, I would check the Security Group for HTTP port 80, then verify the public IP, firewall and network configuration.

## 24. Practical Assignment

Create:

```text
AWS EC2
   ↓
Amazon Linux
   ↓
Apache
   ↓
Custom Website
```

Commands:

```bash
cat /etc/os-release

sudo dnf update -y

sudo dnf install python -y
python --version

sudo dnf install httpd -y

sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

cd /var/www/html
sudo nano index.html
```

Add:

```html
<h1>Tanmay's AWS EC2 Web Server</h1>
<p>I hosted this website using Amazon EC2 and Apache.</p>
```

Test:

```bash
curl http://localhost/
```

Then open:

```text
http://YOUR-PUBLIC-IP/
```

## 25. GitHub Structure

```text
devops-learning/
│
├── README.md
│
├── Linux/
│   ├── Day-01/
│   ├── Day-02/
│   └── Day-03/
│
├── AWS/
│   └── Day-04-EC2/
│       ├── README.md
│       ├── commands.md
│       ├── setup-apache.sh
│       └── screenshots/
│
└── Projects/
```

Never commit your AWS private key.

Recommended `.gitignore`:

```gitignore
*.pem
.env
.venv/
__pycache__/
```

## 26. Day 4 Cheat Sheet

```bash
# OS
cat /etc/os-release

# Update
sudo dnf update -y

# Python
sudo dnf install python -y
python --version

# Apache
sudo dnf install httpd -y

# Service
sudo systemctl start httpd
sudo systemctl stop httpd
sudo systemctl restart httpd
sudo systemctl status httpd
sudo systemctl enable httpd
sudo systemctl disable httpd

# Website
cd /var/www/html
ls -la
sudo touch index.html
sudo nano index.html
cat index.html

# Test
curl http://localhost/
```

## 27. Day 4 Success Checklist

- [ ] Understand cloud computing
- [ ] Know what AWS is
- [ ] Know major cloud providers
- [ ] Understand EC2
- [ ] Understand EC2 instance
- [ ] Understand AMI
- [ ] Understand Amazon Linux
- [ ] Understand SSH
- [ ] Understand key pairs
- [ ] Understand Security Groups
- [ ] Know ports 22, 80 and 443
- [ ] Launch an EC2 instance
- [ ] Connect using SSH
- [ ] Check Amazon Linux
- [ ] Install Python
- [ ] Install Apache/httpd
- [ ] Start Apache
- [ ] Enable Apache
- [ ] Host an HTML website
- [ ] Test using curl
- [ ] Access website using public IP
- [ ] Troubleshoot basic EC2 website problems
- [ ] Understand Stop vs Terminate
- [ ] Never upload .pem to GitHub

## 🔥 Day 4 Interview Summary

Be able to explain this without looking at your notes:

> I launched an Amazon Linux EC2 instance in AWS, created a key pair for SSH authentication, configured a Security Group to allow the required traffic, connected to the server using SSH, installed Apache using the Linux package manager, started and enabled the `httpd` service, created an HTML file under `/var/www/html`, and accessed the website through the EC2 public IP.

## DevOps Learning Progress

```text
Day 01
Linux Fundamentals
       ↓
Day 02
Linux Tools & Package Management
       ↓
Day 03
Apache + Permissions + Bash
       ↓
Day 04
AWS EC2 + SSH + Security Group
+ Apache Web Hosting
       ↓
Day 05
Next Topic
```
