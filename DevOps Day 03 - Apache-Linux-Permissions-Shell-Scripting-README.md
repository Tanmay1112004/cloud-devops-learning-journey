# Day 3 — Apache Web Server, Linux Permissions & Shell Scripting

## 🎯 Learning Objectives

By the end of Day 3, I should understand:

- Linux OS details
- Web server and Apache
- HTTP and HTTPS
- Apache installation on Ubuntu
- `systemctl` service management
- Apache document root
- Linux file permissions
- `chmod`
- Bash shell and shell scripting
- Basic Apache automation
- Basic troubleshooting

---

## 1. Linux OS Details

Check the Linux distribution:

```bash
cat /etc/os-release
```

The `/etc` directory contains many system configuration files.

---

## 2. What is a Web Server?

A web server receives HTTP/HTTPS requests from clients and returns web content or responses.

```text
Browser
   |
   | HTTP Request
   v
Web Server
   |
   v
HTML / CSS / JS
   |
   v
HTTP Response
   |
   v
Browser
```

Examples:

- Apache HTTP Server
- Nginx
- Microsoft IIS
- Caddy

---

## 3. Apache HTTP Server

Apache is an open-source web server.

- Ubuntu/Debian: commonly uses `apache2`
- RHEL-family systems: commonly uses `httpd`

### Common Ports

| Protocol | Port |
|---|---:|
| HTTP | 80 |
| HTTPS | 443 |

---

## 4. Update Ubuntu

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
```

### Difference

```text
apt update
    ↓
Refresh package information

apt list --upgradable
    ↓
Show available upgrades

apt upgrade
    ↓
Install upgrades
```

---

## 5. Install Apache

```bash
sudo apt install apache2 -y
apache2 --version
```

Start Apache:

```bash
sudo systemctl start apache2
```

Check status:

```bash
sudo systemctl status apache2
```

---

## 6. systemctl

`systemctl` is used to manage services controlled by systemd.

```bash
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl status apache2
sudo systemctl enable apache2
sudo systemctl disable apache2
```

### Important

```text
start  → start the service now

enable → start the service automatically during boot
```

Both can be done together:

```bash
sudo systemctl enable --now apache2
```

---

## 7. Apache Document Root

On a standard Ubuntu Apache installation:

```text
/var/www/html
```

Go there:

```bash
cd /var/www/html
pwd
ls -la
```

Typical structure:

```text
/var/www/html/
├── index.html
├── style.css
└── images/
```

Apache serves website files from its configured document root.

---

## 8. Create Your Own Website

Remove the default page:

```bash
sudo rm /var/www/html/index.html
```

Create a new one:

```bash
sudo touch /var/www/html/index.html
sudo nano /var/www/html/index.html
```

Example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Webpage</title>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This website is hosted using Apache.</p>
</body>
</html>
```

Save with:

```text
Ctrl + O
Enter
Ctrl + X
```

Check:

```bash
sudo cat /var/www/html/index.html
```

Open:

```text
http://localhost/
```

> If using a non-default port such as 8080, the URL must include a colon: `http://localhost:8080/`.

---

# 9. Linux File Permissions

Linux permissions control access to files and directories.

Three permission categories:

```text
User / Owner
Group
Others
```

Three basic permissions:

```text
r = Read    = 4
w = Write   = 2
x = Execute = 1
```

Example:

```text
-rwxr-xr-x
```

Breakdown:

```text
-   rwx   r-x   r-x
    |      |     |
   User   Group Others
```

---

## 10. Permission Numbers

### 7

```text
rwx
4 + 2 + 1 = 7
```

### 5

```text
r-x
4 + 1 = 5
```

### 4

```text
r--
4 = 4
```

### 6

```text
rw-
4 + 2 = 6
```

---

## 11. chmod 755

```bash
chmod 755 file
```

Means:

```text
7    5    5
↓    ↓    ↓
rwx  r-x  r-x
```

Full permission:

```text
rwxr-xr-x
```

---

## 12. chmod 644

```bash
chmod 644 file
```

Means:

```text
6    4    4
↓    ↓    ↓
rw-  r--  r--
```

Full permission:

```text
rw-r--r--
```

This is commonly suitable for ordinary website files, depending on ownership and server configuration.

---

## 13. chmod 000

```bash
sudo chmod 000 index.html
```

Removes normal read, write and execute permissions for owner, group and others.

Be careful with this because it can make a file inaccessible through normal permission checks.

---

## 14. echo and Redirection

Print text:

```bash
echo "Hello"
```

Append to a file:

```bash
echo "hello" >> a.txt
```

### `>` vs `>>`

```text
>   → overwrite/truncate
>>  → append
```

Example:

```bash
echo "Line 1" > a.txt
echo "Line 2" >> a.txt
cat a.txt
```

Output:

```text
Line 1
Line 2
```

---

# 15. Bash

Bash = **Bourne Again SHell**

Bash is a popular Linux shell and scripting language.

Architecture:

```text
User
  |
  | command
  v
Bash Shell
  |
  v
Linux Kernel
  |
  v
Hardware
```

Check available shells:

```bash
cat /etc/shells
```

---

# 16. Shell Script

A shell script is a file containing commands that can be executed to automate tasks.

Create:

```bash
touch script.sh
nano script.sh
```

Example:

```bash
#!/bin/bash

echo "Hello"
echo "I am learning DevOps"
```

The first line is called the **shebang** and specifies the interpreter.

Make it executable:

```bash
chmod +x script.sh
```

Run it:

```bash
./script.sh
```

Alternative:

```bash
bash script.sh
```

---

# 17. Apache Automation Script

Basic automation:

```bash
#!/bin/bash

echo "Installing Apache..."

sudo apt update
sudo apt install apache2 -y

echo "Starting Apache..."

sudo systemctl enable --now apache2

echo "Apache setup completed."
```

A more complete version:

```bash
#!/bin/bash

set -e

echo "Updating package information..."
sudo apt update

echo "Installing Apache..."
sudo apt install apache2 -y

echo "Starting Apache..."
sudo systemctl enable --now apache2

echo "Creating website..."

sudo tee /var/www/html/index.html > /dev/null <<'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DevOps Day 3</title>
</head>
<body>
    <h1>Web Server is Running!</h1>
    <p>Apache was installed using a Bash script.</p>
</body>
</html>
EOF

echo "Checking Apache status..."

if systemctl is-active --quiet apache2; then
    echo "Apache is running successfully."
else
    echo "Apache failed to start."
    exit 1
fi

echo "Done!"
```

Run:

```bash
chmod +x setup-apache.sh
./setup-apache.sh
```

---

# 18. Apache Troubleshooting

If the website does not load:

### 1. Check Apache

```bash
sudo systemctl status apache2
```

### 2. Check listening ports

```bash
sudo ss -lntp | grep :80
```

### 3. Test locally

```bash
curl http://localhost/
```

### 4. Check website files

```bash
ls -la /var/www/html
sudo cat /var/www/html/index.html
```

### 5. Check logs

```bash
sudo journalctl -u apache2
```

Apache logs are also under:

```text
/var/log/apache2/
```

---

# 19. Localhost vs AWS EC2

### Local WSL/Linux

```text
Browser
   |
   v
localhost
   |
   v
Apache
   |
   v
/var/www/html
```

### AWS EC2

```text
Your Laptop
     |
  Internet
     |
     v
Public IP
     |
     v
Security Group
     |
     v
Apache
     |
     v
/var/www/html
```

If Apache works locally but not from an EC2 public IP, investigate the Security Group, firewall, networking, Apache listening configuration and public IP.

---

# 20. Important Commands Cheat Sheet

## OS

```bash
cat /etc/os-release
```

## Apache

```bash
sudo apt install apache2 -y
apache2 --version
```

## Service

```bash
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl status apache2
sudo systemctl enable apache2
sudo systemctl disable apache2
```

## Website

```bash
cd /var/www/html
ls -la
sudo nano index.html
sudo cat index.html
```

## Permissions

```bash
chmod 755 file
chmod 644 file
chmod 000 file
chmod +x script.sh
```

## Shell

```bash
cat /etc/shells
bash script.sh
./script.sh
```

## Manual

```bash
man ls
man chmod
man systemctl
man bash
```

---

# 21. Interview Questions

### Q1. What is Apache?

Apache HTTP Server is an open-source web server used to serve web content over HTTP and HTTPS.

### Q2. What is a web server?

A web server receives HTTP/HTTPS requests from clients and returns web resources or responses.

### Q3. What is HTTP's default port?

**80**

### Q4. What is HTTPS's default port?

**443**

### Q5. How do you install Apache on Ubuntu?

```bash
sudo apt update
sudo apt install apache2 -y
```

### Q6. How do you check Apache status?

```bash
sudo systemctl status apache2
```

### Q7. Difference between start and enable?

`start` starts the service now. `enable` configures it to start automatically during boot.

### Q8. What is `/var/www/html`?

It is the default document root for a standard Ubuntu Apache installation.

### Q9. What is chmod?

`chmod` changes file and directory permissions.

### Q10. What does 755 mean?

```text
rwxr-xr-x
```

Owner has read/write/execute; group and others have read/execute.

### Q11. What is Bash?

Bash is a popular Unix/Linux shell and scripting language.

### Q12. What is a shell script?

A file containing shell commands used to execute tasks and automate processes.

### Q13. What is a shebang?

The first line of an executable script that specifies the interpreter, for example:

```bash
#!/bin/bash
```

### Q14. How do you make a script executable?

```bash
chmod +x script.sh
```

### Q15. How do you troubleshoot Apache if a website is not working?

Check service status, listening ports, local connectivity with `curl`, website files, logs, firewall and network configuration.

---

# 22. Day 3 Practical Project

Recommended GitHub structure:

```text
day3-apache/
├── README.md
├── setup-apache.sh
└── website/
    └── index.html
```

The project demonstrates:

- Linux
- Apache
- HTML
- systemd
- File permissions
- Bash
- Automation
- Troubleshooting

---

# 23. Day 3 Success Checklist

- [ ] Understand web server
- [ ] Understand Apache
- [ ] Know HTTP/HTTPS ports
- [ ] Install Apache
- [ ] Start/stop/restart Apache
- [ ] Understand `systemctl`
- [ ] Understand `start` vs `enable`
- [ ] Host an HTML page
- [ ] Understand `/var/www/html`
- [ ] Understand `r/w/x`
- [ ] Calculate `755`
- [ ] Calculate `644`
- [ ] Use `chmod`
- [ ] Understand Bash
- [ ] Create a `.sh` script
- [ ] Use a shebang
- [ ] Make a script executable
- [ ] Automate Apache setup
- [ ] Troubleshoot Apache

---

## DevOps Connection

The key lesson of Day 3:

```text
Linux
  ↓
Install Software
  ↓
Apache
  ↓
Service Management
  ↓
Website
  ↓
Permissions
  ↓
Bash
  ↓
Automation
```

**DevOps mindset: Don't just perform the task manually—learn how to automate it.**
