# 🐧 DevOps Day 09 — Linux System Administration

> **Course:** Cloud Computing & DevOps  
> **Topic:** Linux System Administration  
> **Main Areas:** Processes, Services, Packages, Logs, Backups, Cron Jobs  
> **Goal:** Learn how a DevOps/System Administrator manages and troubleshoots a Linux server.

---

# 📚 Table of Contents

1. Linux System Administration
2. Process Management
3. PID and Parent Processes
4. `ps`, `top`, and `htop`
5. Finding and Killing Processes
6. Background and Foreground Jobs
7. Process Tree and Resource Monitoring
8. Process Creation with `fork()`
9. Linux Boot Process
10. systemd
11. Service Management
12. Nginx and Apache
13. Ports and Port Conflicts
14. journalctl and System Logs
15. Linux Package Management
16. RPM, DEB and SUSE Families
17. Installing Programming Languages
18. Backup and Archival
19. tar
20. zip
21. Important Linux Directories
22. Log Management
23. Cron Jobs
24. Cron Syntax
25. Cron Practical
26. Troubleshooting
27. DevOps Interview Questions
28. Hands-On Practice
29. GitHub Structure
30. Quick Revision

---

# 1. What is Linux System Administration?

**Linux System Administration** is the process of managing, maintaining, configuring and troubleshooting Linux systems.

A Linux administrator or DevOps engineer commonly performs tasks such as:

- Managing processes
- Managing services
- Installing software
- Monitoring CPU and memory
- Managing users
- Checking logs
- Performing backups
- Scheduling tasks
- Managing permissions
- Troubleshooting applications
- Monitoring disk usage
- Managing networking
- Maintaining servers

---

# 🌍 Linux Administration in DevOps

```text
Linux Server
     │
     ├── Processes
     ├── Services
     ├── Users
     ├── Files
     ├── Packages
     ├── Logs
     ├── Backups
     ├── Networking
     └── Scheduled Jobs
            │
            ↓
      DevOps Automation
```

Linux administration is one of the foundational skills required for DevOps.

---

# 2. What is a Process?

A **process is a running instance of a program**.

For example, a program exists as a file on disk.

When you execute it, Linux creates a process.

```text
Program
   ↓
Execute
   ↓
Process
   ↓
CPU + Memory + PID
```

Examples of processes:

- Bash
- Python program
- Nginx
- Apache
- SSH server
- Jenkins
- Docker daemon

---

# 3. Program vs Process

| Program | Process |
|---|---|
| Passive file | Running instance |
| Stored on disk | Loaded into memory |
| Not executing | Currently executing |
| Example: `/usr/bin/python3` | Example: running Python script |

Example:

```bash
python3 hello.py
```

`python3` is a program.

The running execution of Python becomes a **process**.

---

# 4. PID — Process ID

Every Linux process receives a numeric identifier known as:

> **PID — Process ID**

Example:

```text
nginx → PID 1250
bash  → PID 1480
ssh   → PID 900
```

PIDs allow Linux administrators to identify and control individual processes.

---

# 5. What is PID 1?

A common classroom statement is:

> "Root process ID is always 1."

A better explanation is:

**PID 1 is the first userspace process started by the Linux kernel.**

On most modern Linux distributions it is:

```text
systemd
```

Check:

```bash
ps -p 1
```

or:

```bash
ps -fp 1
```

Typical output:

```text
UID     PID  PPID CMD
root      1     0 /usr/lib/systemd/systemd
```

So:

```text
Linux Kernel
     ↓
PID 1
     ↓
systemd
     ↓
Other services/processes
```

PID 1 usually runs as `root`, but saying "root is PID 1" is inaccurate because **root is a user account**, while PID is a process identifier.

---

# 6. Parent Process and Child Process

Processes can create other processes.

The creating process is the:

> Parent Process

The created process is the:

> Child Process

Every process can therefore have:

```text
PID  = Process ID
PPID = Parent Process ID
```

Example:

```text
systemd (PID 1)
   │
   ├── sshd
   │     └── bash
   │           └── python3
   │
   ├── nginx
   │
   └── crond
```

---

# 7. `ps` Command

`ps` means:

> Process Status

Basic command:

```bash
ps
```

It normally displays processes associated with the current terminal/session.

---

# 8. View All Processes

A very common command:

```bash
ps aux
```

Typical output:

```text
USER       PID %CPU %MEM   VSZ   RSS COMMAND
root         1  0.0  0.2  ...   ... systemd
root       850  0.0  0.4  ...   ... sshd
ec2-user  1420  0.0  0.1  ...   ... bash
```

Important columns:

| Column | Meaning |
|---|---|
| USER | User running process |
| PID | Process ID |
| %CPU | CPU usage |
| %MEM | Memory usage |
| VSZ | Virtual memory |
| RSS | Physical memory |
| COMMAND | Process command |

---

# 9. `ps -a`

```bash
ps -a
```

This does **not** mean:

> "Background process is ON."

It displays processes associated with terminals, subject to the options used.

For complete process inspection, DevOps engineers commonly use:

```bash
ps aux
```

or:

```bash
ps -ef
```

---

# 10. Manual Page

To study `ps`:

```bash
man ps
```

Manual pages are very useful while working on Linux servers.

---

# 11. `top`

`top` provides real-time process monitoring.

```bash
top
```

It shows:

- CPU usage
- Memory usage
- Load average
- Running processes
- Process IDs
- Process states

Press:

```text
q
```

to quit.

---

# 12. `htop`

`htop` is an interactive and visually improved process monitoring tool.

Install on RPM-based systems where available:

```bash
sudo dnf install htop -y
```

or:

```bash
sudo yum install htop -y
```

Run:

```bash
htop
```

It provides:

- Colored output
- CPU graphs
- Memory graphs
- Interactive sorting
- Process searching
- Process killing

---

# 13. top vs htop

| `top` | `htop` |
|---|---|
| Usually already installed | May need installation |
| Basic terminal interface | Interactive interface |
| Keyboard-driven | Easier navigation |
| Excellent on servers | User-friendly monitoring |

---

# 14. `pidof`

Find PID by exact program name:

```bash
pidof nginx
```

Example:

```text
1452 1451
```

Some services create multiple worker processes, therefore you may see multiple PIDs.

---

# 15. `pgrep`

Search processes by name or pattern:

```bash
pgrep nginx
```

Example:

```bash
pgrep sshd
```

Useful command:

```bash
pgrep -a nginx
```

which can display PID and command information.

---

# 16. `pidof` vs `pgrep`

| Command | Purpose |
|---|---|
| `pidof` | Find PIDs of a named program |
| `pgrep` | Search process table using a pattern |

Example:

```bash
pidof nginx
```

```bash
pgrep nginx
```

---

# 17. Killing Processes

Linux provides several ways to stop processes.

---

## `kill`

```bash
kill PID
```

Example:

```bash
kill 1250
```

By default, this normally sends:

```text
SIGTERM (15)
```

SIGTERM asks the process to terminate gracefully.

---

# 18. Force Kill

```bash
kill -9 PID
```

Example:

```bash
kill -9 1250
```

This sends:

```text
SIGKILL (9)
```

SIGKILL immediately terminates the process.

### Important

Do not use `kill -9` as your first option.

Prefer:

```bash
kill PID
```

first.

Use `kill -9` only when graceful termination fails.

---

# 19. `pkill`

Kill processes by name or pattern:

```bash
sudo pkill nginx
```

---

# 20. `killall`

Example:

```bash
sudo killall httpd
```

It attempts to send a signal to processes matching the supplied program name.

Be careful when using process-name based termination on production servers.

---

# 21. Important Linux Signals

| Signal | Number | Purpose |
|---|---:|---|
| SIGTERM | 15 | Graceful termination |
| SIGKILL | 9 | Force termination |
| SIGSTOP | 19/varies | Stop/pause |
| SIGCONT | 18/varies | Continue stopped process |

The exact numeric values of some signals can be architecture-dependent, so the symbolic names are safest when discussing them.

---

# 22. Process Jobs

Linux shells support foreground and background jobs.

Run a command in background:

```bash
sleep 100 &
```

List jobs:

```bash
jobs
```

Bring a job to foreground:

```bash
fg %1
```

Continue a stopped job in background:

```bash
bg %1
```

---

# 23. Process Tree

Use:

```bash
pstree
```

Example:

```text
systemd─┬─sshd───bash───python3
        ├─httpd
        ├─crond
        └─docker
```

This helps understand parent-child relationships.

---

# 24. Memory Monitoring

Use:

```bash
free -m
```

or:

```bash
free -h
```

`-h` is often easier because values are human-readable.

Example:

```text
              total    used    free
Mem:           3.8Gi   1.2Gi   1.7Gi
Swap:             0B      0B      0B
```

---

# 25. `vmstat`

Your classroom notes contained:

```text
vimstat 5
```

Correct command:

```bash
vmstat 5
```

`vmstat` reports information about:

- Processes
- Memory
- Paging
- CPU
- System activity

`5` means refresh every five seconds.

---

# 26. Other Monitoring Commands

```bash
uptime
```

Shows:

- Uptime
- Logged-in users
- Load averages

---

```bash
date
```

Shows date and time.

---

```bash
whoami
```

Shows current user.

---

```bash
hostname
```

Shows hostname.

---

# 27. Process Creation with `fork()`

Your classroom C example contained syntax errors.

A correct example is:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    fork();

    printf("PID: %d, Parent PID: %d\n",
           getpid(),
           getppid());

    sleep(60);

    return 0;
}
```

Save:

```bash
nano fork.c
```

Install compiler:

```bash
sudo dnf install gcc -y
```

or:

```bash
sudo yum install gcc -y
```

Compile:

```bash
gcc fork.c -o fork
```

Run:

```bash
./fork
```

Because:

```c
fork();
```

creates a child process, both parent and child continue executing the following code.

You may therefore see two lines.

---

# 28. Understanding `fork()`

Initially:

```text
Parent Process
```

After:

```c
fork();
```

```text
          Parent
         /      \
    Parent      Child
```

Both continue from the instruction following `fork()`.

---

# 29. Why Process Management Matters in DevOps

Imagine Jenkins is not responding.

A DevOps engineer may check:

```bash
systemctl status jenkins
```

then:

```bash
ps aux | grep jenkins
```

then:

```bash
pgrep -a java
```

then resource usage:

```bash
top
```

then logs:

```bash
journalctl -u jenkins
```

This is real DevOps troubleshooting.

---

# ⚙️ SYSTEM INITIALIZATION

# 30. What is System Initialization?

System initialization is the sequence Linux follows while booting and preparing the operating system and its services.

Typical modern Linux boot flow:

```text
Power ON
   ↓
BIOS / UEFI
   ↓
Bootloader (GRUB)
   ↓
Linux Kernel
   ↓
systemd
   ↓
Services
   ↓
Login / Applications
```

---

# 31. BIOS / UEFI

BIOS/UEFI initializes hardware and locates a bootable device.

---

# 32. Bootloader

A common Linux bootloader is:

```text
GRUB
```

GRUB loads the Linux kernel.

---

# 33. Kernel

The kernel initializes:

- CPU
- Memory
- Drivers
- Filesystems
- Hardware interfaces

It then starts the first userspace process.

---

# 34. systemd

On most modern Linux systems:

```text
systemd
```

becomes PID 1.

Its responsibilities include:

- Starting services
- Stopping services
- Managing dependencies
- Boot targets
- Logging integration
- Service supervision

---

# 35. systemctl

`systemctl` communicates with systemd.

Basic syntax:

```bash
systemctl ACTION SERVICE
```

---

# 36. Start a Service

```bash
sudo systemctl start httpd
```

---

# 37. Stop a Service

```bash
sudo systemctl stop httpd
```

---

# 38. Restart a Service

```bash
sudo systemctl restart httpd
```

---

# 39. Check Service Status

```bash
sudo systemctl status httpd
```

---

# 40. Enable at Boot

```bash
sudo systemctl enable httpd
```

This configures the service to start automatically during appropriate future boots.

---

# 41. Disable at Boot

```bash
sudo systemctl disable httpd
```

Important:

```bash
stop
```

and:

```bash
disable
```

are different.

| Command | Effect |
|---|---|
| `stop` | Stops running service now |
| `disable` | Prevents automatic startup |
| `start` | Starts service now |
| `enable` | Configures startup at boot |

---

# 42. Useful Combined Command

Enable and immediately start:

```bash
sudo systemctl enable --now httpd
```

---

# 🌐 NGINX AND APACHE

# 43. Install Nginx

Depending on distribution/repository:

```bash
sudo dnf install nginx -y
```

or:

```bash
sudo yum install nginx -y
```

Check version:

```bash
nginx -v
```

Start properly under systemd:

```bash
sudo systemctl start nginx
```

Enable:

```bash
sudo systemctl enable nginx
```

Status:

```bash
sudo systemctl status nginx
```

---

# 44. Apache Web Server

Package/service names depend on the Linux family.

### RHEL / CentOS / Amazon Linux

Common package/service:

```text
httpd
```

Install:

```bash
sudo dnf install httpd -y
```

Start:

```bash
sudo systemctl start httpd
```

---

### Ubuntu / Debian

Common package/service:

```text
apache2
```

Install:

```bash
sudo apt install apache2 -y
```

Start:

```bash
sudo systemctl start apache2
```

---

# 45. Apache vs Nginx and Port 80

Both commonly listen on:

```text
TCP Port 80
```

for HTTP.

It is not accurate to say:

> "Apache and Nginx can never run together."

They **can run together**, but they cannot normally both bind to the exact same:

```text
IP + Port
```

at the same time.

Example conflict:

```text
Nginx → 0.0.0.0:80
Apache → 0.0.0.0:80
```

This will fail.

But this can work:

```text
Nginx  → Port 80
Apache → Port 8081
```

or they can be configured on different IP addresses.

---

# 46. Check Which Process Uses a Port

Useful modern command:

```bash
sudo ss -tulpn
```

Filter port 80:

```bash
sudo ss -tulpn | grep :80
```

This is extremely useful in DevOps troubleshooting.

---

# 47. Access Web Server

For a server/EC2 instance:

```text
http://PUBLIC-IP
```

Example:

```text
http://54.x.x.x
```

For this to work, you typically need:

- Web server running
- Port 80 listening
- Host firewall allowing traffic where applicable
- AWS Security Group allowing inbound TCP 80

---

# 📜 JOURNALCTL

# 48. What is journalctl?

`journalctl` displays logs collected by the systemd journal.

Basic:

```bash
journalctl
```

---

# 49. Service-Specific Logs

Nginx:

```bash
journalctl -u nginx
```

Apache:

```bash
journalctl -u httpd
```

SSH:

```bash
journalctl -u sshd
```

Jenkins:

```bash
journalctl -u jenkins
```

Docker:

```bash
journalctl -u docker
```

---

# 50. Live Logs

```bash
journalctl -f
```

`-f` means follow.

Conceptually similar to:

```bash
tail -f
```

---

# 51. Current Boot Logs

```bash
journalctl -b
```

Useful when debugging problems that occurred during system startup.

---

# 52. Recent Service Logs

A very useful command:

```bash
journalctl -u nginx -n 50
```

Shows recent entries.

Follow live:

```bash
journalctl -u nginx -f
```

---

# 53. DevOps Troubleshooting Workflow

Suppose Nginx fails.

Start with:

```bash
sudo systemctl status nginx
```

Then:

```bash
journalctl -u nginx
```

Then:

```bash
sudo ss -tulpn | grep :80
```

Then check configuration:

```bash
sudo nginx -t
```

This is a realistic troubleshooting sequence.

---

# 📦 SOFTWARE AND PACKAGE MANAGEMENT

# 54. Linux Distribution Families

A better way to understand your classroom "3 types" point is through major distribution/package families.

---

## RPM Family

Examples:

- Red Hat Enterprise Linux
- CentOS Stream
- Fedora
- Amazon Linux
- Rocky Linux
- AlmaLinux

Package format:

```text
.rpm
```

Tools commonly include:

```text
rpm
yum
dnf
```

---

# 55. Debian Family

Examples:

- Debian
- Ubuntu
- Linux Mint

Package format:

```text
.deb
```

Tools:

```text
dpkg
apt
```

---

# 56. SUSE Family

Examples:

- SUSE Linux Enterprise
- openSUSE

Common tool:

```text
zypper
```

---

# 57. Package Manager Overview

| Linux Family | Package Manager |
|---|---|
| Debian / Ubuntu | `apt` |
| RHEL / Fedora / Amazon Linux | `dnf` / `yum` |
| SUSE | `zypper` |

---

# 58. Update Packages

RHEL-family:

```bash
sudo dnf update -y
```

or older systems:

```bash
sudo yum update -y
```

Ubuntu/Debian:

```bash
sudo apt update
sudo apt upgrade -y
```

---

# 59. Important Correction — Upgradeable Packages

Your notes contain:

```bash
sudo yum list --upgradable
```

This is not the normal command.

For Ubuntu:

```bash
apt list --upgradable
```

For yum:

```bash
yum check-update
```

For dnf:

```bash
dnf check-upgrade
```

---

# 60. Search Packages

Example:

```bash
dnf search gcc
```

or:

```bash
yum search gcc
```

---

# 61. Install Package

```bash
sudo dnf install git -y
```

or:

```bash
sudo yum install git -y
```

---

# 62. Remove Package

```bash
sudo dnf remove nginx -y
```

or:

```bash
sudo yum remove nginx -y
```

---

# 63. RPM

RPM means:

> Red Hat Package Manager

List installed RPM packages:

```bash
rpm -qa
```

Find specific package:

```bash
rpm -qa | grep nginx
```

---

# 64. Programming Language Practice

Your class suggested writing Hello World programs in multiple languages.

This is useful because DevOps engineers work with applications built using different technology stacks.

---

## Python

Check:

```bash
python3 --version
```

Create:

```bash
nano hello.py
```

Code:

```python
print("Hello from Python")
```

Run:

```bash
python3 hello.py
```

---

# 65. Go

Install where available:

```bash
sudo dnf install golang -y
```

or the package may be named `go` depending on distribution/repository.

Check:

```bash
go version
```

Your notes contain:

```bash
go -v
```

That is not the standard version-check command.

Use:

```bash
go version
```

---

# 66. C

Install compiler:

```bash
sudo dnf install gcc -y
```

Check:

```bash
gcc --version
```

Compile:

```bash
gcc hello.c -o hello
```

Run:

```bash
./hello
```

---

# 67. Java

Check:

```bash
java --version
```

Java package names depend heavily on Linux distribution and repository.

For tools such as Jenkins, always verify the currently supported Java version for the Jenkins version you install.

---

# 🤖 JENKINS INTRODUCTION

# 68. What is Jenkins?

Jenkins is an automation server commonly used for:

- Continuous Integration
- Continuous Delivery
- Automated builds
- Automated testing
- Deployment pipelines

Typical architecture:

```text
Developer
    ↓
Git Repository
    ↓
Jenkins
    ↓
Build
    ↓
Test
    ↓
Deploy
```

---

# 69. Jenkins Service

Once properly installed:

```bash
sudo systemctl start jenkins
```

Enable:

```bash
sudo systemctl enable jenkins
```

Status:

```bash
sudo systemctl status jenkins
```

Logs:

```bash
journalctl -u jenkins
```

---

# 70. Jenkins Default Port

Jenkins commonly uses:

```text
8080
```

Access format:

```text
http://PUBLIC-IP:8080
```

Notice the colon:

```text
:
```

Your original notes contained:

```text
PUBLIC-IP.8080
```

which is incorrect.

Correct:

```text
PUBLIC-IP:8080
```

---

# 71. AWS Security Group for Jenkins

If Jenkins runs on EC2 and listens on port 8080, the instance's networking/security configuration must allow required inbound access.

For a controlled learning lab:

```text
Protocol: TCP
Port:     8080
Source:   your trusted IP where possible
```

Avoid unnecessarily exposing administrative interfaces such as Jenkins to the entire internet.

---

# 72. Important Jenkins Installation Rule

Do not permanently copy a version-specific Jenkins/Java installation procedure into your notes as if it never changes.

Jenkins versions, repository configuration and supported Java releases can change.

Your long-term note should be:

```text
1. Install supported Java
2. Add official Jenkins repository
3. Install Jenkins
4. daemon-reload if required
5. Start Jenkins
6. Enable Jenkins
7. Check status
8. Check logs
9. Allow required network access
```

---

# 🐳 Docker as a Service Example

# 73. Docker Service

If Docker is installed:

```bash
sudo systemctl start docker
```

Enable:

```bash
sudo systemctl enable docker
```

Status:

```bash
sudo systemctl status docker
```

Logs:

```bash
journalctl -u docker
```

This demonstrates the same Linux service-management concepts.

---

# 💾 BACKUP AND ARCHIVAL

# 74. Backup vs Archive vs Compression

These are related but not identical concepts.

### Backup

A copy of important data used for recovery.

### Archive

Combines multiple files/directories into one file.

### Compression

Reduces the size of data.

---

# 75. tar

`tar` is commonly used to create archives.

Think:

```text
a.txt
b.txt
folder/
    ↓
tar
    ↓
backup.tar
```

---

# 76. Create tar Archive

```bash
tar -cvf new.tar a.txt b.txt
```

Meaning:

```text
c = create
v = verbose
f = archive file
```

---

# 77. Extract tar Archive

```bash
tar -xvf new.tar
```

Meaning:

```text
x = extract
v = verbose
f = archive file
```

Your class also used:

```bash
tar -xf new.tar
```

which is valid.

`v` is optional.

---

# 78. Archive Directory

```bash
tar -cvf myfile.tar a.txt b.txt new/
```

Extract:

```bash
tar -xvf myfile.tar
```

---

# 79. tar.gz Compression

A plain:

```text
.tar
```

archive is not necessarily compressed.

Compressed archive:

```bash
tar -czvf backup.tar.gz folder/
```

Here:

```text
z = gzip compression
```

Extract:

```bash
tar -xzvf backup.tar.gz
```

---

# 80. tar Options

| Option | Meaning |
|---|---|
| `c` | Create |
| `x` | Extract |
| `v` | Verbose |
| `f` | Archive file |
| `z` | gzip |
| `t` | List archive contents |

Example:

```bash
tar -tvf backup.tar
```

---

# 81. zip

Create ZIP archive:

```bash
zip new.zip a.txt b.txt
```

Extract:

```bash
unzip new.zip
```

---

# 82. Zip a Directory

Your notes show examples similar to:

```bash
zip backup.zip /tmp/
```

For directories, use recursive mode:

```bash
zip -r backup.zip /tmp/
```

Without `-r`, ZIP will not recursively include directory contents.

---

# 83. tar vs zip

| tar | zip |
|---|---|
| Excellent on Linux | Cross-platform |
| Archives files/directories | Archives + compression |
| Often combined with gzip | Compression built into ZIP workflow |
| Preserves Unix metadata well | Common for file sharing |

---

# 84. rsync

A very useful backup/synchronization tool is:

```bash
rsync
```

Example:

```bash
rsync -av source/ destination/
```

It is widely used for:

- Directory synchronization
- Backups
- Remote transfers
- Incremental copying

---

# 85. Important Backup Locations

Examples:

```text
/etc
/home
/var/www
application data
database backups
configuration files
```

---

# 86. `/etc`

Contains system and application configuration.

Examples:

```text
/etc/ssh/
/etc/nginx/
/etc/httpd/
/etc/passwd
```

---

# 87. `/home`

Contains normal user home directories.

Example:

```text
/home/ec2-user
/home/tanmay
```

---

# 88. `/var/www`

Common location used for web content on some Linux web-server setups.

Exact web roots depend on distribution and configuration.

---

# 89. Backup Rule

A backup is useful only when:

```text
Backup Created
     ↓
Backup Verified
     ↓
Restore Tested
```

A backup that cannot be restored is not a reliable backup.

---

# 📜 LOG MANAGEMENT

# 90. What are Logs?

Logs are records of events generated by:

- Operating system
- Services
- Applications
- Authentication systems
- Security tools

Logs are essential for:

- Troubleshooting
- Monitoring
- Auditing
- Security investigation
- Incident analysis

---

# 91. Common Log Directory

```bash
cd /var/log
```

Then:

```bash
ls -la
```

---

# 92. View a Log

```bash
cat dnf.log
```

For larger files, prefer:

```bash
less dnf.log
```

---

# 93. `head`

Show beginning:

```bash
head dnf.log
```

Default:

```text
first 10 lines
```

Specify amount:

```bash
head -n 20 dnf.log
```

---

# 94. `tail`

Show end:

```bash
tail dnf.log
```

Specify:

```bash
tail -n 20 dnf.log
```

---

# 95. Live Log Monitoring

```bash
tail -f logfile
```

Example on systems using `/var/log/syslog`:

```bash
tail -f /var/log/syslog
```

Not every Linux distribution uses `/var/log/syslog`.

For example, RHEL-family distributions commonly use other files and/or systemd journal.

---

# 96. Search Logs with grep

```bash
grep "error" logfile
```

Case-insensitive:

```bash
grep -i "error" logfile
```

Example:

```bash
grep -i "failed" /var/log/secure
```

where applicable.

---

# 97. journalctl vs Traditional Log Files

Linux systems may use both:

```text
Traditional files
        +
systemd journal
```

Examples:

```bash
tail -f /var/log/messages
```

and:

```bash
journalctl -f
```

Availability depends on distribution and configuration.

---

# 98. Log Rotation

Logs continuously grow and can consume disk space.

Linux commonly uses:

```text
logrotate
```

Main configuration:

```text
/etc/logrotate.conf
```

Additional configuration often appears under:

```text
/etc/logrotate.d/
```

---

# ⏰ CRON JOBS

# 99. What is Cron?

**Cron is a time-based job scheduler in Linux used to automatically execute commands or scripts at specific times or intervals.**

Common examples:

- Backup every night
- Delete temporary files
- Generate reports
- Run monitoring scripts
- Synchronize files
- Perform maintenance

---

# 100. Cron Architecture

```text
Cron Service
   │
   │ reads
   ↓
Crontab
   │
   │ contains
   ↓
Schedule + Command
   │
   ↓
Automatic Execution
```

---

# 101. `cron` vs `crond`

Service naming depends on Linux distribution.

### RHEL / CentOS / Amazon Linux

Usually:

```text
crond
```

### Ubuntu / Debian

Usually:

```text
cron
```

---

# 102. Install Cron — Amazon Linux 2023

```bash
sudo dnf install cronie -y
```

Then:

```bash
sudo systemctl enable --now crond
```

Check:

```bash
sudo systemctl status crond
```

---

# 103. Important Correction

Your classroom notes contain:

```bash
sudo systemctl start crontab
sudo systemctl enable crontab
```

On RHEL/Amazon-Linux style systems this is generally incorrect.

Use:

```bash
sudo systemctl start crond
sudo systemctl enable crond
```

`crontab` is the command used to edit/manage user schedules.

`crond` is the background daemon/service.

---

# 104. Edit Cron Jobs

```bash
crontab -e
```

---

# 105. List Cron Jobs

```bash
crontab -l
```

---

# 106. Cron Syntax

Basic:

```text
* * * * * command
```

Diagram:

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └──── Day of Week
│ │ │ └────── Month
│ │ └──────── Day of Month
│ └────────── Hour
└──────────── Minute
```

---

# 107. Cron Fields

| Position | Field | Values |
|---:|---|---|
| 1 | Minute | 0–59 |
| 2 | Hour | 0–23 |
| 3 | Day of month | 1–31 |
| 4 | Month | 1–12 |
| 5 | Day of week | commonly 0–7 |

Sunday is commonly represented by:

```text
0
```

and on many cron implementations also:

```text
7
```

---

# 108. Every Minute

```cron
* * * * * command
```

---

# 109. Every 5 Minutes

```cron
*/5 * * * * command
```

---

# 110. Every Hour

```cron
0 * * * * command
```

Meaning:

```text
00 minutes of every hour
```

---

# 111. Every Day at 2 AM

```cron
0 2 * * * command
```

---

# 112. Every Sunday at Midnight

```cron
0 0 * * 0 command
```

---

# 113. Monday-Friday at 8:30 AM

```cron
30 8 * * 1-5 command
```

---

# 114. Cron Date Practical

Open:

```bash
crontab -e
```

Add:

```cron
* * * * * date >> /home/ec2-user/date.log 2>&1
```

This runs every minute.

Check configured jobs:

```bash
crontab -l
```

Check output:

```bash
cat /home/ec2-user/date.log
```

---

# 115. Understanding Redirection

Consider:

```bash
date >> date.log 2>&1
```

### `>>`

Append standard output.

### `2>`

Redirect standard error.

### `2>&1`

Redirect standard error to the same destination as standard output.

So:

```bash
command >> app.log 2>&1
```

stores both normal output and errors.

---

# 116. Cron Environment

An important interview concept:

Cron does **not** necessarily run with the exact same environment as your interactive Bash shell.

For reliability, prefer absolute paths.

Instead of:

```cron
0 2 * * * backup.sh
```

prefer:

```cron
0 2 * * * /home/ec2-user/backup.sh
```

---

# 117. Cron Backup Job

Make script executable:

```bash
chmod +x /home/ec2-user/backup.sh
```

Then:

```bash
crontab -e
```

Add:

```cron
0 2 * * * /home/ec2-user/backup.sh >> /home/ec2-user/backup.log 2>&1
```

This executes daily at 2 AM.

---

# 118. Important Cron Mistake

Do not type:

```text
0 2 * * * /backup.sh
```

directly into Bash.

It belongs inside:

```bash
crontab -e
```

Otherwise Bash interprets:

```text
0
```

as a command.

---

# 119. Cron Troubleshooting

If a job does not run:

### 1. Check service

Amazon/RHEL:

```bash
sudo systemctl status crond
```

Ubuntu:

```bash
sudo systemctl status cron
```

### 2. Check schedule

```bash
crontab -l
```

### 3. Check script permissions

```bash
ls -l /home/ec2-user/script.sh
```

### 4. Make executable

```bash
chmod +x /home/ec2-user/script.sh
```

### 5. Run manually

```bash
/home/ec2-user/script.sh
```

### 6. Use absolute paths

### 7. Redirect output/errors to a log file

---

# 🔥 DEVOPS TROUBLESHOOTING SCENARIOS

# 120. Scenario — Website is Down

Suppose your website is unreachable.

Check:

```text
1. Is server running?
        ↓
2. Is web service running?
        ↓
3. Is process running?
        ↓
4. Is port listening?
        ↓
5. Are logs showing errors?
        ↓
6. Is firewall/security group allowing traffic?
        ↓
7. Is disk/memory exhausted?
```

Commands:

```bash
systemctl status nginx
```

```bash
pgrep nginx
```

```bash
sudo ss -tulpn | grep :80
```

```bash
journalctl -u nginx
```

```bash
df -h
```

```bash
free -h
```

---

# 121. Scenario — Process Consuming High CPU

Check:

```bash
top
```

or:

```bash
htop
```

Identify PID.

Then:

```bash
ps -fp PID
```

Investigate before terminating.

If appropriate:

```bash
kill PID
```

Only if necessary:

```bash
kill -9 PID
```

---

# 122. Scenario — Service Won't Start

Check:

```bash
systemctl status SERVICE
```

Then:

```bash
journalctl -u SERVICE
```

Check:

- Configuration syntax
- Port conflict
- Permissions
- Missing dependency
- Disk space
- Memory
- Environment variables

---

# 🎤 INTERVIEW QUESTIONS

# ⭐ Basic Interview Questions

## Q1. What is a process?

A process is a running instance of a program.

---

## Q2. What is PID?

PID stands for Process ID. It uniquely identifies a running process.

---

## Q3. What is PPID?

PPID is the Process ID of the parent process that created the current process.

---

## Q4. What is usually PID 1 on modern Linux?

`systemd`.

---

## Q5. What does `ps aux` do?

It displays detailed information about running processes.

---

## Q6. What is `top`?

A real-time process and resource monitoring utility.

---

## Q7. Difference between top and htop?

`htop` provides a more interactive and user-friendly process-monitoring interface, while `top` is more basic and commonly preinstalled.

---

## Q8. What does `pidof` do?

It returns process IDs for a specified program name.

---

## Q9. What does `pgrep` do?

It searches running processes using names or patterns.

---

## Q10. What is `kill -9`?

It sends SIGKILL, forcing a process to terminate immediately.

---

# 🔥 Intermediate Questions

## Q11. Difference between SIGTERM and SIGKILL?

SIGTERM requests graceful termination.

SIGKILL immediately terminates the process and cannot be caught or ignored by the target process.

---

## Q12. What is systemd?

systemd is the init/service manager used by many modern Linux distributions.

---

## Q13. What is systemctl?

A command used to control and inspect systemd units/services.

---

## Q14. Difference between start and enable?

`start` runs a service now.

`enable` configures it for automatic startup during future boots.

---

## Q15. What is journalctl?

A command for querying logs stored in the systemd journal.

---

## Q16. How do you view live system logs?

```bash
journalctl -f
```

---

## Q17. How do you view logs for Nginx?

```bash
journalctl -u nginx
```

and application-specific files may also exist under:

```text
/var/log/nginx/
```

---

## Q18. What is RPM?

RPM is the package format/package-management system used by Red Hat-family Linux distributions.

---

## Q19. Difference between yum and dnf?

Both manage RPM-based packages. DNF is the newer package-management framework used by modern Fedora/RHEL-family distributions.

---

## Q20. What is tar?

A utility for combining multiple files and directories into an archive.

---

# 🔥 Cron Interview Questions

## Q21. What is Cron?

Cron is a time-based job scheduler used for automatically executing commands or scripts.

---

## Q22. What is crontab?

A utility/file mechanism used to define scheduled Cron jobs.

---

## Q23. What are the five Cron fields?

```text
Minute
Hour
Day of Month
Month
Day of Week
```

---

## Q24. What does this mean?

```cron
*/5 * * * *
```

Every five minutes.

---

## Q25. What does this mean?

```cron
0 2 * * *
```

Every day at 2:00 AM.

---

## Q26. Difference between cron and crond?

On Linux distributions that use the `crond` name, `crond` is the background daemon that executes Cron jobs.

---

## Q27. Why can a script work manually but fail in Cron?

Possible reasons:

- Different PATH
- Limited environment
- Relative paths
- Permissions
- Wrong user
- Missing environment variables
- Script errors
- Cron daemon not running

This is a very common DevOps interview question.

---

# 💼 Scenario-Based Interview Questions

## Q28. Nginx isn't starting. What will you do?

Check:

```bash
systemctl status nginx
journalctl -u nginx
nginx -t
ss -tulpn
df -h
```

Then investigate configuration errors, port conflicts and resource issues.

---

## Q29. Your server is slow. What commands would you use?

```bash
top
htop
free -h
vmstat
df -h
ps aux
```

---

## Q30. Port 80 is already in use. How will you identify the process?

```bash
sudo ss -tulpn | grep :80
```

---

## Q31. How would you schedule a backup every night at 2 AM?

```cron
0 2 * * * /home/ec2-user/backup.sh
```

---

## Q32. A cron job isn't executing. How do you troubleshoot?

Check:

```text
Cron daemon
    ↓
Crontab entry
    ↓
Script manually
    ↓
Permissions
    ↓
Absolute paths
    ↓
Environment
    ↓
Logs
```

---

# 🧪 HANDS-ON PRACTICE

## Task 1 — Process Inspector

Run:

```bash
ps aux
```

Find:

- PID 1
- Current Bash PID
- SSH PID
- Web-server PID

---

## Task 2 — Background Process

Run:

```bash
sleep 300 &
```

Find it with:

```bash
jobs
pgrep sleep
ps aux | grep sleep
```

Then stop it gracefully.

---

## Task 3 — Resource Monitor

Practice:

```bash
top
free -h
vmstat 5
uptime
df -h
```

Explain what each command tells you.

---

## Task 4 — Apache Lab

Install Apache/httpd.

Start it.

Enable it.

Check status.

Check PID.

Check port 80.

View logs.

Stop it.

Disable it.

---

## Task 5 — Nginx Port Conflict

With Apache already using port 80:

1. Start Nginx.
2. Observe failure if both are configured for the same endpoint.
3. Check:

```bash
systemctl status nginx
```

4. Check:

```bash
journalctl -u nginx
```

5. Check:

```bash
ss -tulpn | grep :80
```

Understand **why** the service failed.

---

## Task 6 — Process Creation

Compile the `fork.c` program.

Run:

```bash
./fork
```

During the 60-second sleep, execute:

```bash
pstree
```

and:

```bash
ps aux | grep fork
```

Observe parent and child processes.

---

## Task 7 — tar Practice

Create:

```text
a.txt
b.txt
new/
```

Archive:

```bash
tar -cvf files.tar a.txt b.txt new/
```

Delete originals.

Restore:

```bash
tar -xvf files.tar
```

---

## Task 8 — Compressed Backup

Create:

```bash
tar -czvf backup.tar.gz myfolder/
```

List archive:

```bash
tar -tzvf backup.tar.gz
```

Extract:

```bash
tar -xzvf backup.tar.gz
```

---

## Task 9 — Log Analysis

Navigate:

```bash
cd /var/log
```

Practice:

```bash
ls -lah
head logfile
tail logfile
tail -f logfile
grep -i "error" logfile
```

---

## Task 10 — Cron Lab

Create:

```bash
nano cron-test.sh
```

Write:

```bash
#!/bin/bash

echo "Cron executed at $(date)" >> /home/ec2-user/cron-test.log
```

Make executable:

```bash
chmod +x cron-test.sh
```

Add to:

```bash
crontab -e
```

```cron
* * * * * /home/ec2-user/cron-test.sh
```

Verify:

```bash
cat /home/ec2-user/cron-test.log
```

---

# 🚀 MINI PROJECT — Linux Server Health Monitor

Create:

```text
server-health.sh
```

It should display:

```text
=================================
        SERVER HEALTH REPORT
=================================

Hostname:
Date:
Uptime:
Current User:

CPU / Load:
Memory Usage:
Disk Usage:

Top Processes:
Running Services:
Listening Ports:
=================================
```

Useful commands:

```bash
hostname
date
uptime
whoami
free -h
df -h
ps aux
systemctl
ss -tulpn
```

This is an excellent beginner DevOps project.

---

# 🚀 MINI PROJECT — Automated Backup

Create:

```text
backup.sh
```

Requirements:

1. Backup selected directory.
2. Add current date to filename.
3. Compress using tar.gz.
4. Save to backup folder.
5. Print success/failure.
6. Write output to log file.
7. Schedule using Cron.

Architecture:

```text
Source Directory
      ↓
backup.sh
      ↓
tar + gzip
      ↓
backup_DATE.tar.gz
      ↓
Backup Directory
      ↓
Cron runs automatically
```

This begins combining:

```text
Linux
+
Bash
+
tar
+
Cron
+
Logging
=
DevOps Automation
```

---

# 📁 GITHUB STRUCTURE FOR DAY 09

Do not create another repository.

Keep using:

```text
cloud-computing-devops-learning
```

Recommended:

```text
cloud-computing-devops-learning/
│
├── 01-Linux/
│   │
│   ├── Notes/
│   │   ├── Day-08-Bash-Scripting.md
│   │   └── Day-09-Linux-System-Administration.md
│   │
│   ├── Process-Management/
│   │   ├── commands.md
│   │   └── fork.c
│   │
│   ├── Service-Management/
│   │   └── systemd-commands.md
│   │
│   ├── Package-Management/
│   │   └── package-manager-cheatsheet.md
│   │
│   ├── Backup/
│   │   ├── tar-practice.md
│   │   └── backup.sh
│   │
│   ├── Logs/
│   │   └── log-management.md
│   │
│   ├── Cron/
│   │   ├── cron-notes.md
│   │   └── cron-test.sh
│   │
│   └── Projects/
│       ├── server-health.sh
│       └── automated-backup.sh
│
├── Interview-Questions/
│   └── Linux-Interview-Questions.md
│
└── README.md
```

---

# ⚡ DAY 09 COMMAND CHEAT SHEET

## Processes

```bash
ps
ps aux
ps -ef
top
htop
pidof nginx
pgrep nginx
pstree
jobs
fg
bg
kill PID
kill -9 PID
pkill nginx
killall nginx
```

---

## Resources

```bash
free -h
vmstat 5
uptime
df -h
```

---

## Services

```bash
systemctl status SERVICE
sudo systemctl start SERVICE
sudo systemctl stop SERVICE
sudo systemctl restart SERVICE
sudo systemctl enable SERVICE
sudo systemctl disable SERVICE
sudo systemctl enable --now SERVICE
```

---

## Logs

```bash
journalctl
journalctl -u SERVICE
journalctl -u SERVICE -f
journalctl -b
journalctl -f

head logfile
tail logfile
tail -f logfile
grep -i "error" logfile
```

---

## Packages

```bash
dnf search PACKAGE
dnf install PACKAGE
dnf remove PACKAGE
dnf update
dnf check-upgrade

yum search PACKAGE
yum install PACKAGE
yum remove PACKAGE
yum update
yum check-update

rpm -qa
```

---

## Backup

```bash
tar -cvf backup.tar directory/
tar -xvf backup.tar

tar -czvf backup.tar.gz directory/
tar -xzvf backup.tar.gz

zip files.zip a.txt b.txt
zip -r backup.zip directory/
unzip backup.zip

rsync -av source/ destination/
```

---

## Cron

```bash
sudo dnf install cronie -y
sudo systemctl enable --now crond
sudo systemctl status crond

crontab -e
crontab -l
```

---

# 🧠 DAY 09 QUICK REVISION

Remember:

```text
Process
  = Running program

PID
  = Process ID

PPID
  = Parent Process ID

PID 1
  = Usually systemd

ps
  = Process information

top / htop
  = Live process monitoring

pidof / pgrep
  = Find process

kill
  = Send signal to process

SIGTERM
  = Graceful termination

SIGKILL
  = Forced termination

systemd
  = Init/service manager

systemctl
  = Manage systemd services

journalctl
  = Read systemd journal logs

RPM
  = Red Hat package format/system

dnf / yum
  = RPM-family package managers

apt
  = Debian-family package manager

tar
  = Archive

gzip
  = Compression

cron
  = Time-based scheduler

crond
  = Cron daemon on many RHEL-style systems

crontab
  = Manage user cron schedules
```

---

# 🔥 DAY 09 MUST-KNOW INTERVIEW TOPICS

You should be able to explain without notes:

1. What is a process?
2. Program vs process
3. PID and PPID
4. What is PID 1?
5. `ps aux`
6. `top` vs `htop`
7. `pidof` vs `pgrep`
8. SIGTERM vs SIGKILL
9. Parent vs child process
10. What does `fork()` do?
11. Linux boot process
12. What is systemd?
13. What is systemctl?
14. `start` vs `enable`
15. What is journalctl?
16. How to debug a failed service
17. What is RPM?
18. yum vs dnf vs apt
19. tar vs zip
20. Backup vs archive
21. What are logs?
22. `tail -f`
23. What is Cron?
24. Cron syntax
25. cron vs crond vs crontab
26. Why cron works differently from interactive shell
27. How to troubleshoot a Cron job
28. How to identify which process owns a port
29. How to diagnose a slow server
30. How to troubleshoot a website that is down

---

# 🎯 The Most Important Day 09 Concept

Do not treat today's class as a collection of random Linux commands.

The actual relationship is:

```text
Linux Server
     │
     ├── Processes
     │      ↓
     │   ps / top / kill
     │
     ├── Services
     │      ↓
     │   systemd / systemctl
     │
     ├── Logs
     │      ↓
     │   journalctl / tail / grep
     │
     ├── Software
     │      ↓
     │   dnf / yum / apt
     │
     ├── Data
     │      ↓
     │   tar / zip / rsync
     │
     └── Automation
            ↓
          cron
            ↓
       Bash Scripts
            ↓
          DevOps
```

A good DevOps engineer should be able to answer:

> **"My application isn't working. What is happening on the Linux server underneath it?"**

Day 08 taught you how to **automate Linux using Bash**.

Day 09 teaches you how to **administer and troubleshoot the Linux system itself**.

Together:

```text
Linux Fundamentals
       +
Bash Scripting
       +
System Administration
       ↓
Strong DevOps Foundation
```