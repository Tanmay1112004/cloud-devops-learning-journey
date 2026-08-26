# Day 3 — Apache Web Server, Linux Permissions & Shell Scripting

## 🎯 Learning Objectives

By the end of Day 3, you should understand:

- Linux OS details
- Web servers
- Apache HTTP Server
- HTTP and HTTPS
- Installing Apache on Ubuntu
- `systemctl` service management
- Apache document root
- Hosting a basic HTML website
- Linux file permissions
- `chmod`
- Bash and shell scripting
- Automating Apache installation
- Basic Apache troubleshooting

---

## 1. Check Linux OS Details

```bash
cat /etc/os-release
```

This displays information about the Linux distribution.

Example:

```text
NAME="Ubuntu"
ID=ubuntu
```

### Why is this useful?

Before installing software on a server, identify the Linux distribution because package-management commands differ.

```text
Ubuntu / Debian  → apt
RHEL-family      → dnf
```

---

## 2. What is a Web Server?

A web server is software that receives HTTP/HTTPS requests from clients and returns web content or responses.

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

Apache HTTP Server is an open-source web server used to host websites and web applications.

Official website:

https://httpd.apache.org/

### Apache naming

```text
Ubuntu / Debian
      |
   apache2

RHEL-family
      |
    httpd
```

---

## 4. HTTP and HTTPS

### HTTP

HTTP = Hypertext Transfer Protocol

Default port:

```text
80
```

### HTTPS

HTTPS = Hypertext Transfer Protocol Secure

Default port:

```text
443
```

```text
Browser
   |
   +---- HTTP  → Port 80
   |
   +---- HTTPS → Port 443
```

---

## 5. Update Ubuntu

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
```

### Remember

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

## 6. Install Apache

```bash
sudo apt install apache2 -y
```

Check version:

```bash
apache2 --version
```

---

## 7. Manage Apache with systemctl

### Start

```bash
sudo systemctl start apache2
```

### Stop

```bash
sudo systemctl stop apache2
```

### Restart

```bash
sudo systemctl restart apache2
```

### Check status

```bash
sudo systemctl status apache2
```

### Enable at boot

```bash
sudo systemctl enable apache2
```

### Disable at boot

```bash
sudo systemctl disable apache2
```

### Important

`start` and `enable` are different:

```text
start  → start the service now
enable → start automatically during boot
```

To do both:

```bash
sudo systemctl enable --now apache2
```

---

## 8. Apache Document Root

On a standard Ubuntu Apache installation:

```text
/var/www/html
```

Navigate:

```bash
cd /var/www/html
pwd
ls -la
```

The directory normally contains the files Apache serves as website content.

```text
Apache
   |
   v
/var/www/html/
   |
   +-- index.html
   +-- style.css
   +-- images/
```

---

## 9. Create Your Own Website

Remove the default page if needed:

```bash
sudo rm /var/www/html/index.html
```

Create a new file:

```bash
sudo touch /var/www/html/index.html
sudo nano /var/www/html/index.html
```

Example HTML:

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
    <p>This is my first website hosted using Apache.</p>

</body>
</html>
```

Save in nano:

```text
Ctrl + O
Enter
Ctrl + X
```

Check:

```bash
sudo cat /var/www/html/index.html
```

---

## 10. Open the Website

For Apache listening on the default HTTP port:

```text
http://localhost/
```

If a service is configured to use port 8080:

```text
http://localhost:8080/
```

The colon is required before a custom port.

---

## 11. What is localhost?

`localhost` refers to the local machine.

Usually:

```text
localhost → 127.0.0.1
```

Architecture:

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

---

## 12. Linux File Permissions

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

## 13. Permission Numbers

### 7

```text
rwx
4 + 2 + 1 = 7
```

### 6

```text
rw-
4 + 2 = 6
```

### 5

```text
r-x
4 + 1 = 5
```

### 4

```text
r--
4
```

---

## 14. chmod 755

```bash
chmod 755 file
```

Means:

```text
7 = rwx
5 = r-x
5 = r-x
```

Therefore:

```text
rwxr-xr-x
```

Owner:

- Read
- Write
- Execute

Group:

- Read
- Execute

Others:

- Read
- Execute

---

## 15. chmod 644

```bash
chmod 644 file
```

Means:

```text
6 = rw-
4 = r--
4 = r--
```

Result:

```text
rw-r--r--
```

This is a common permission pattern for ordinary files.

---

## 16. chmod 000

```bash
chmod 000 file
```

Removes read, write and execute permissions from user, group and others.

```text
---------
```

Use carefully because it can make a file inaccessible through normal permissions.

---

## 17. chmod +x

To make a shell script executable:

```bash
chmod +x script.sh
```

Check:

```bash
ls -la script.sh
```

---

## 18. echo and Redirection

Print text:

```bash
echo "Hello"
```

Append text to a file:

```bash
echo "hello" >> a.txt
```

### `>` vs `>>`

```text
>   → overwrite/truncate and write
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

# 19. What is Bash?

Bash = Bourne Again SHell.

Bash is a commonly used Linux/Unix shell and scripting language.

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

Bash can be used to:

- Execute commands
- Run programs
- Use variables
- Use conditions
- Use loops
- Automate tasks
- Create shell scripts

---

## 20. What is a Shell Script?

A shell script is a file containing shell commands that can be executed to automate tasks.

Example:

```bash
#!/bin/bash

echo "Hello"
echo "I am learning DevOps"
```

---

## 21. Shebang

The first line:

```bash
#!/bin/bash
```

is called a shebang.

It specifies the interpreter used to execute the script when the script is run as an executable.

---

## 22. Create and Run a Shell Script

Create:

```bash
touch script.sh
```

Edit:

```bash
nano script.sh
```

Add:

```bash
#!/bin/bash

echo "Hello"
echo "I am learning Shell Scripting"
```

Make executable:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

Alternative:

```bash
bash script.sh
```

---

## 23. Apache Automation Script

Basic automation:

```bash
#!/bin/bash

echo "Installing Apache..."

sudo apt update
sudo apt install apache2 -y

echo "Starting Apache..."

sudo systemctl start apache2
sudo systemctl enable apache2

echo "Apache setup completed."
```

This demonstrates an important DevOps concept:

> Automate repetitive server setup instead of performing every step manually.

---

## 24. Improved Apache Automation Script

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

sudo systemctl --no-pager status apache2

echo "Done!"
```

---

## 25. Why Use `set -e`?

```bash
set -e
```

This tells Bash to exit when a command fails in common situations.

Conceptually:

```text
Command 1 → success
Command 2 → success
Command 3 → failure
                 |
                 v
              Stop
```

This helps prevent a script from blindly continuing after an important failure.

---

## 26. Important Script Correction

This:

```bash
echo "<h1>Web Server</h1>"
```

only prints HTML to the terminal.

It does NOT write the HTML to `index.html`.

To write it:

```bash
echo "<h1>Web Server</h1>" | sudo tee /var/www/html/index.html
```

Then:

```bash
sudo cat /var/www/html/index.html
```

---

# 27. Apache Troubleshooting

If the website doesn't load:

### Step 1 — Check Apache

```bash
sudo systemctl status apache2
```

### Step 2 — Check listening port

```bash
sudo ss -lntp | grep :80
```

### Step 3 — Test locally

```bash
curl http://localhost/
```

### Step 4 — Check website files

```bash
ls -la /var/www/html
sudo cat /var/www/html/index.html
```

### Step 5 — Check logs

```bash
sudo journalctl -u apache2
```

Apache logs:

```text
/var/log/apache2/
```

---

# 28. Localhost vs AWS EC2

On WSL/local Linux:

```text
Browser
   |
   v
localhost
   |
   v
Apache
```

On AWS EC2:

```text
Your Laptop
     |
     | Internet
     v
AWS EC2 Public IP
     |
     v
Security Group
     |
     | Port 80
     v
Apache
     |
     v
/var/www/html
```

If Apache works locally but not through an EC2 public IP, investigate:

- Security Group
- Firewall
- Apache listening configuration
- Public IP/DNS
- Network/routing

---

# 29. Useful `man` Commands

Linux provides manual pages.

```bash
man ls
man cat
man chmod
man systemctl
man bash
```

`man` helps you learn command syntax and options.

---

# 30. Important Day 3 Commands Cheat Sheet

### OS

```bash
cat /etc/os-release
```

### Apache

```bash
sudo apt install apache2 -y
apache2 --version
```

### Service management

```bash
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl status apache2
sudo systemctl enable apache2
sudo systemctl disable apache2
sudo systemctl enable --now apache2
```

### Website

```bash
cd /var/www/html
ls -la
sudo nano index.html
sudo cat index.html
```

### Permissions

```bash
chmod 755 file
chmod 644 file
chmod 000 file
chmod +x script.sh
```

### Shell

```bash
cat /etc/shells
bash script.sh
./script.sh
```

### Documentation

```bash
man ls
man chmod
man systemctl
```

---

# 31. Interview Questions

## Q1. What is Apache?

**Answer:**

Apache HTTP Server is an open-source web server used to serve web content over HTTP and HTTPS.

## Q2. What is the default HTTP port?

**Answer:**

Port `80`.

## Q3. What is the default HTTPS port?

**Answer:**

Port `443`.

## Q4. How do you install Apache on Ubuntu?

```bash
sudo apt update
sudo apt install apache2 -y
```

## Q5. How do you check Apache status?

```bash
sudo systemctl status apache2
```

## Q6. Difference between `start` and `enable`?

**Answer:**

`start` starts the service immediately. `enable` configures the service to start automatically during boot.

## Q7. What is `/var/www/html`?

**Answer:**

On a standard Ubuntu Apache configuration, it is the default document root from which Apache serves website files.

## Q8. What is `chmod`?

**Answer:**

`chmod` means change mode and is used to modify Linux file and directory permissions.

## Q9. What does `755` mean?

```text
7 = rwx
5 = r-x
5 = r-x

rwxr-xr-x
```

## Q10. What is Bash?

**Answer:**

Bash is the Bourne Again Shell, a commonly used Linux shell and scripting language.

## Q11. What is a shell script?

**Answer:**

A shell script is a file containing shell commands used to automate tasks.

## Q12. What is `#!/bin/bash`?

**Answer:**

It is a shebang that specifies Bash as the interpreter for the script when executed as an executable.

## Q13. How do you make a script executable?

```bash
chmod +x script.sh
```

## Q14. How do you execute it?

```bash
./script.sh
```

or:

```bash
bash script.sh
```

---

# 32. Scenario Interview Question

### Apache is installed but the website isn't loading. What will you do?

Strong answer:

> "First, I would check whether Apache is running using `systemctl status apache2`. Then I would verify that Apache is listening on port 80, test the service locally using curl, check the document root and website files, and review Apache logs. If this is an AWS EC2 server, I would also check the Security Group, firewall and network configuration."

---

# 33. Day 3 Practical Project

Create:

```text
day3-apache/
├── README.md
├── setup-apache.sh
└── website/
    └── index.html
```

Your script should:

1. Check OS
2. Update packages
3. Install Apache
4. Start Apache
5. Enable Apache
6. Create/copy `index.html`
7. Check Apache status
8. Test localhost

---

# 34. Day 3 Success Checklist

- [ ] I understand what a web server is.
- [ ] I can explain Apache.
- [ ] I know HTTP = 80.
- [ ] I know HTTPS = 443.
- [ ] I can install Apache.
- [ ] I can start/stop/restart Apache.
- [ ] I understand `systemctl`.
- [ ] I understand `start` vs `enable`.
- [ ] I know `/var/www/html`.
- [ ] I can create an HTML webpage.
- [ ] I can access it through localhost.
- [ ] I understand Linux permissions.
- [ ] I can calculate `755`.
- [ ] I can calculate `644`.
- [ ] I understand `chmod`.
- [ ] I understand Bash.
- [ ] I can create a `.sh` script.
- [ ] I understand the shebang.
- [ ] I can make a script executable.
- [ ] I can automate Apache installation.
- [ ] I can perform basic Apache troubleshooting.

---

# 35. Day 3 DevOps Learning Chain

```text
Linux
  ↓
Package Manager
  ↓
Install Software
  ↓
Apache
  ↓
systemd / systemctl
  ↓
Web Server
  ↓
HTML
  ↓
Linux Permissions
  ↓
Bash
  ↓
Shell Script
  ↓
Automation
```

## Key DevOps Mindset

> Don't just perform a task manually. Learn how to automate it, repeat it, and troubleshoot it.

---

## Day Progress

```text
Day 1 → Linux Fundamentals
Day 2 → Tools + Package Management
Day 3 → Apache + Permissions + Bash
Day 4 → Next Topic
```
