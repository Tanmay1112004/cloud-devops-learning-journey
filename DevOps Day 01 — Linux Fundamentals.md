# DevOps Day 01 — Linux Fundamentals

> **Course:** Cloud Computing & DevOps\
> **Institute:** Ethans Tech Pune\
> **Topic:** Linux Fundamentals\
> **Goal:** Understand Linux basics, WSL, distributions, essential commands, package management, SSH, and basic Python execution.

---

## 1. Learning Objectives

By the end of this session, I should be able to:

- Explain what Linux is.
- Understand Linux vs Unix.
- Explain the Linux kernel.
- Understand Linux distributions.
- Install and use Ubuntu through WSL.
- Navigate the Linux filesystem.
- Create files and directories.
- Work with hidden files.
- Read and edit files.
- Install packages using `apt`.
- Run Python programs from Linux.
- Understand basic networking ports.
- Explain why Linux is important in DevOps.

---

# 2. What is Linux?

Linux is a **free and open-source operating system kernel**.

In everyday conversation, people commonly call complete Linux-based operating systems "Linux," but technically:

- **Linux = kernel**
- **Distribution = Linux kernel + system utilities + package manager + applications + other components**

Examples of Linux distributions:

- Ubuntu
- Debian
- RHEL
- Fedora
- Rocky Linux
- AlmaLinux
- Arch Linux
- Amazon Linux

Linux is widely used in:

- Cloud computing
- Data centers
- Web servers
- DevOps
- Cybersecurity
- Scientific computing
- Supercomputers
- Embedded systems
- Containers

---

# 3. Who Created Linux?

Linux was created by **Linus Torvalds** in **1991**.

Linus Torvalds initially developed the Linux kernel while he was a student.

Linux was inspired by the Unix operating-system family.

### Interview Answer

**Interviewer:** Who created Linux?

**Answer:**

> "Linux was created by Linus Torvalds in 1991. It is an open-source, Unix-like kernel that became the foundation of many Linux distributions such as Ubuntu, Debian and RHEL."

---

# 4. Linux and Unix

Linux is **Unix-like**, but Linux is not the original Unix operating system.

### Unix

Unix is an older family of operating systems that became influential in servers and enterprise computing.

### Linux

Linux is an open-source Unix-like kernel that was created by Linus Torvalds.

Simple way to remember:

```text
Unix
  │
  ├── Inspired many Unix-like systems
  │
  ▼
Linux
  │
  ├── Ubuntu
  ├── Debian
  ├── RHEL
  ├── Fedora
  └── Amazon Linux
```

---

# 5. Linux Kernel

The **kernel** is the core component of an operating system.

It acts as a bridge between applications and computer hardware.

```text
+--------------------------------+
|          Applications          |
| Python | Git | Docker | Nginx |
+--------------------------------+
               |
               ▼
+--------------------------------+
|          Linux Kernel          |
| Process | Memory | Files | Net |
+--------------------------------+
               |
               ▼
+--------------------------------+
|            Hardware            |
| CPU | RAM | Disk | Network     |
+--------------------------------+
```

The kernel manages important resources such as:

- CPU
- Memory
- Processes
- Devices
- Filesystems
- Networking

### Interview Question

**What is a kernel?**

**Answer:**

> "The kernel is the core part of an operating system. It manages hardware resources such as CPU, memory and devices and provides an interface between applications and hardware."

---

# 6. Why Linux is Important in DevOps

Linux is extremely important in DevOps because many:

- Cloud servers
- Web servers
- CI/CD systems
- Containers
- Kubernetes nodes
- Monitoring systems
- Automation tools

run on Linux.

For example:

```text
Developer
    |
    ▼
GitHub
    |
    ▼
CI/CD
    |
    ▼
Cloud
    |
    ▼
Linux Server
    |
    ├── Docker
    ├── Kubernetes
    ├── Nginx
    ├── Python
    └── Monitoring
```

Therefore, Linux is one of the **most important foundations for a DevOps engineer**.

---

# 7. Why Choose Linux?

Important advantages:

### 1. Open Source

The source code is available for modification and distribution according to its license.

### 2. Stable

Linux is widely used for long-running server workloads.

### 3. Secure

Linux provides permissions, users, groups and other security mechanisms.

### 4. Multi-user

Multiple users can work on the same Linux system with different permissions.

### 5. Multitasking

Linux can run multiple processes/programs at the same time.

> **Important:** Multiple programs running at the same time is called **multitasking**, not batch processing.

### 6. Automation Friendly

Linux provides powerful shells and command-line tools that are heavily used for automation.

### 7. Cloud Friendly

Many cloud virtual machines use Linux-based operating systems.

---

# 8. Linux Distributions

A Linux distribution is a complete operating system built around the Linux kernel.

Examples:

| Distribution | Family / Base   | Common Usage                |
| ------------ | --------------- | --------------------------- |
| Ubuntu       | Debian          | Development, servers, cloud |
| Debian       | Debian          | Servers, general purpose    |
| RHEL         | Red Hat         | Enterprise                  |
| Fedora       | Red Hat         | Development                 |
| Rocky Linux  | RHEL-compatible | Enterprise/server           |
| Amazon Linux | AWS-focused     | AWS workloads               |
| Arch Linux   | Independent     | Advanced users              |

---

# 9. Ubuntu

Ubuntu is one of the most popular Linux distributions.

It is **Debian-based**.

```text
Debian
   |
   └── Ubuntu
```

Ubuntu is commonly used for:

- Development
- Cloud servers
- Learning Linux
- DevOps
- Web servers

---

# 10. RHEL

**RHEL = Red Hat Enterprise Linux**

It is an enterprise Linux distribution developed by Red Hat.

It is commonly used in:

- Enterprises
- Data centers
- Production servers
- Large organizations

---

# 11. Linux Distribution Reference

You can explore different Linux distributions at:

[DistroWatch](https://distrowatch.com?utm_source=chatgpt.com)

Use it as a reference to learn about different distributions.

---

# 12. WSL

## WSL = Windows Subsystem for Linux

WSL allows you to run a Linux environment directly on Windows without needing a traditional dual-boot setup.

Basic architecture:

```text
Windows
   |
   ▼
WSL
   |
   ▼
Ubuntu
   |
   ├── Linux commands
   ├── Bash
   ├── Python
   ├── Git
   └── Linux tools
```

This is very useful for learning Linux on a Windows laptop.

---

# 13. Installing WSL

Open **PowerShell as Administrator**.

Run:

```bash
wsl --install
```

If you need to specify Ubuntu:

```bash
wsl --install -d Ubuntu
```

After installation, you may need to restart Windows.

Then launch WSL:

```bash
wsl
```

You should now be inside your Linux environment.

---

# 14. Check Linux Information

Run:

```bash
cat /etc/os-release
```

This displays information about the Linux distribution.

Example:

```text
NAME="Ubuntu"
VERSION="..."
ID=ubuntu
```

---

# 15. Linux Command Line

Linux provides a powerful command-line interface.

Instead of clicking graphical menus, we can perform operations using commands.

For example:

```bash
pwd
ls
cd
mkdir
touch
cat
```

This is particularly important in DevOps because cloud servers are frequently managed remotely through the command line.

---

# 16. Essential Linux Commands

## `pwd`

`pwd` means:

**Print Working Directory**

It shows your current location.

```bash
pwd
```

Example:

```text
/home/tanmay
```

---

## `ls`

Lists files and directories.

```bash
ls
```

---

## `ls -la`

Lists files and directories, including hidden files.

```bash
ls -la
```

The `-a` option means **all**, including hidden entries.

The `-l` option gives a detailed/long listing.

---

# 17. Hidden Files

In Linux, files and directories whose names begin with `.` are hidden by default.

Example:

```bash
mkdir .new
```

Creates a hidden directory.

Create a hidden file:

```bash
touch .b.txt
```

Check it:

```bash
ls
```

It may not appear.

Now:

```bash
ls -la
```

You can see it.

Example:

```text
.
..
.new
.b.txt
a.txt
```

---

# 18. `mkdir`

`mkdir` means:

**Make Directory**

Example:

```bash
mkdir myfolder
```

Creates:

```text
myfolder/
```

---

# 19. `touch`

`touch` can create an empty file.

Example:

```bash
touch a.txt
```

Create a hidden file:

```bash
touch .b.txt
```

---

# 20. `cd`

`cd` means:

**Change Directory**

Example:

```bash
cd myfolder
```

Move to the parent directory:

```bash
cd ..
```

Example:

```text
/home/tanmay/project
                  |
                  | cd ..
                  ▼
/home/tanmay
```

---

# 21. `cat`

`cat` is commonly used to display file contents.

Example:

```bash
cat a.txt
```

If `a.txt` contains:

```text
Hello Linux
```

then:

```bash
cat a.txt
```

outputs:

```text
Hello Linux
```

---

# 22. `nano`

`nano` is a simple terminal text editor.

Open a file:

```bash
nano a.txt
```

You can type/edit the content.

Common Nano shortcuts:

```text
Ctrl + O    Save
Ctrl + X    Exit
```

> Note: In Nano, **Ctrl+O** is the save/write-out shortcut. It is not Ctrl+S.

---

# 23. `tree`

`tree` displays directories and files in a tree structure.

Install it on Ubuntu:

```bash
sudo apt install tree -y
```

Then:

```bash
tree
```

Example:

```text
.
├── project
│   └── helloworld.py
├── a.txt
└── b.txt
```

---

# 24. `sudo`

`sudo` means:

**Superuser Do**

It allows an authorized user to execute commands with elevated privileges.

Example:

```bash
sudo apt update
```

Think of it as:

```text
Normal User
     |
     | sudo
     ▼
Elevated privileges
```

Use `sudo` carefully.

---

# 25. `apt`

`apt` is a package-management command used by Debian-based distributions such as Ubuntu.

For example:

```bash
sudo apt update
```

Updates the package information.

Install Python:

```bash
sudo apt install python3 -y
```

Install Git:

```bash
sudo apt install git -y
```

Install Tree:

```bash
sudo apt install tree -y
```

---

# 26. What Does `-y` Mean?

Example:

```bash
sudo apt install git -y
```

`-y` automatically answers **yes** to prompts that ask for confirmation.

Without it, the package manager may ask:

```text
Do you want to continue? [Y/n]
```

With `-y`, it automatically confirms.

---

# 27. Update vs Upgrade

Two commands you should understand:

```bash
sudo apt update
```

and:

```bash
sudo apt upgrade
```

### `apt update`

Refreshes the package information/index.

### `apt upgrade`

Actually upgrades installed packages to newer versions.

Simple memory trick:

```text
update  → refresh information
upgrade → upgrade installed software
```

---

# 28. Install Python

On Ubuntu, use:

```bash
sudo apt update
sudo apt install python3 -y
```

Check:

```bash
python3 --version
```

Depending on the Ubuntu configuration, the `python` command may or may not point to Python 3.

If necessary, Ubuntu provides:

```bash
sudo apt install python-is-python3
```

Then:

```bash
python --version
```

---

# 29. Install Git

```bash
sudo apt update
sudo apt install git -y
```

Check:

```bash
git --version
```

---

# 30. Linux Filesystem Basics

Linux has a hierarchical filesystem.

The top-level directory is:

```text
/
```

This is called the **root directory**.

Example:

```text
/
├── home
│   └── tanmay
│       ├── project
│       └── notes
│
├── etc
├── var
├── tmp
├── usr
└── bin
```

Don't confuse:

```text
/
```

with:

```text
/root
```

`/` is the filesystem root.

`/root` is the home directory of the root user.

---

# 31. Important Linux Paths

Some common directories:

| Directory | Purpose                        |
| --------- | ------------------------------ |
| `/`       | Root of filesystem             |
| `/home`   | Normal users' home directories |
| `/root`   | Root user's home               |
| `/etc`    | Configuration files            |
| `/var`    | Variable data such as logs     |
| `/tmp`    | Temporary files                |
| `/usr`    | User-space programs and data   |
| `/bin`    | Essential commands             |

You don't need to memorize all of them on Day 1. Understand the concept first.

---

# 32. SSH

## SSH = Secure Shell

SSH is used to securely connect to and manage remote systems.

Default SSH port:

```text
22
```

Example:

```text
Your Laptop
     |
     | SSH : 22
     ▼
Linux Cloud Server
```

This is extremely important for DevOps.

For example, you can connect from your laptop to an AWS EC2 Linux instance using SSH.

---

# 33. Common Network Ports

| Service | Protocol | Common Port |
| ------- | -------- | ----------: |
| SSH     | SSH      |          22 |
| HTTP    | HTTP     |          80 |
| HTTPS   | HTTPS    |         443 |

### HTTP

**HTTP = Hypertext Transfer Protocol**

Default port:

```text
80
```

### HTTPS

**HTTPS = Hypertext Transfer Protocol Secure**

Default port:

```text
443
```

HTTPS provides encrypted communication using TLS.

---

# 34. AWS + Linux

Linux knowledge becomes very important when working with AWS.

Typical workflow:

```text
AWS Console
     |
     ▼
Launch EC2
     |
     ▼
Linux Virtual Machine
     |
     ▼
Connect using SSH
     |
     ▼
Linux Terminal
     |
     ├── Install packages
     ├── Configure server
     ├── Run applications
     ├── Check logs
     └── Automate tasks
```

This is why Linux is one of the first technologies you should become comfortable with for DevOps.

---

# 35. Practical Assignment 1 — Directory Structure

Create this structure:

```text
.new/
└── hello/
    ├── a.txt
    └── b.txt
```

Commands:

```bash
mkdir .new
cd .new
mkdir hello
cd hello
touch a.txt
touch b.txt
```

Check:

```bash
ls
```

Then:

```bash
cd ../..
ls -la
```

You should be able to see `.new`.

---

# 36. Practical Assignment 2 — Verify with Tree

Install tree:

```bash
sudo apt install tree -y
```

Then:

```bash
tree -a
```

The `-a` option allows hidden files/directories to be displayed.

Expected concept:

```text
.
└── .new
    └── hello
        ├── a.txt
        └── b.txt
```

---

# 37. Practical Assignment 3 — Create and Read a File

Create:

```bash
touch a.txt
```

Edit:

```bash
nano a.txt
```

Write:

```text
Hello, I am learning Linux.
```

Save and exit.

Then:

```bash
cat a.txt
```

Expected:

```text
Hello, I am learning Linux.
```

---

# 38. Practical Assignment 4 — Python Hello World

Create a project:

```bash
mkdir project
cd project
```

Create the Python file:

```bash
touch helloworld.py
```

Edit:

```bash
nano helloworld.py
```

Write:

```python
print("Hello World")
```

Save and exit.

Run:

```bash
python3 helloworld.py
```

Expected output:

```text
Hello World
```

If `python` is configured for Python 3:

```bash
python helloworld.py
```

---

# 39. Complete Day 1 Practice

Try this yourself without looking at the answers.

### Task 1

Create:

```text
devops/
└── linux/
    ├── notes.txt
    ├── commands.txt
    └── scripts/
```

### Task 2

Create a hidden directory:

```text
.devops
```

### Task 3

Inside it create:

```text
config.txt
```

### Task 4

Write something into `notes.txt` using Nano.

### Task 5

Display the contents using:

```bash
cat notes.txt
```

### Task 6

Display the complete structure:

```bash
tree -a
```

### Task 7

Create and execute a Python program that prints:

```text
I am learning DevOps
```

---

# 40. Important Commands — Day 1 Cheat Sheet

| Command               | Purpose                          |
| --------------------- | -------------------------------- |
| `pwd`                 | Show current directory           |
| `ls`                  | List files/directories           |
| `ls -la`              | List all including hidden files  |
| `cd`                  | Change directory                 |
| `cd ..`               | Go to parent directory           |
| `mkdir`               | Create directory                 |
| `touch`               | Create empty file                |
| `cat`                 | Display file contents            |
| `nano`                | Edit a file                      |
| `tree`                | Show directory tree              |
| `sudo`                | Execute with elevated privileges |
| `apt`                 | Manage packages                  |
| `python3 --version`   | Check Python version             |
| `git --version`       | Check Git version                |
| `cat /etc/os-release` | Check Linux distribution         |
| `wsl`                 | Start WSL                        |
| `ssh`                 | Connect to remote system         |

---

# 41. Command Practice

You should be able to understand these commands:

```bash
pwd

ls

ls -la

mkdir myfolder

cd myfolder

touch a.txt

nano a.txt

cat a.txt

cd ..

sudo apt update

sudo apt install python3 -y

python3 --version

sudo apt install git -y

git --version

cat /etc/os-release
```

Don't just memorize them.

**Type them yourself in WSL.**

---

# 42. Interview Questions — Beginner

## Q1. What is Linux?

**Answer:**

> "Linux is a free and open-source Unix-like operating system kernel. Linux-based distributions such as Ubuntu and RHEL are widely used for servers, cloud computing and DevOps."

---

## Q2. Who created Linux?

**Answer:**

> "Linux was created by Linus Torvalds in 1991."

---

## Q3. What is a Linux distribution?

**Answer:**

> "A Linux distribution is a complete operating system built around the Linux kernel along with system utilities, package managers and applications. Examples include Ubuntu, Debian and RHEL."

---

## Q4. What is the Linux kernel?

**Answer:**

> "The Linux kernel is the core component of the operating system. It manages resources such as CPU, memory, processes, devices and networking."

---

## Q5. What is WSL?

**Answer:**

> "WSL stands for Windows Subsystem for Linux. It allows us to run a Linux environment directly on Windows without needing a traditional dual-boot setup."

---

## Q6. What is the difference between `ls` and `ls -la`?

**Answer:**

> "`ls` lists files and directories, while `ls -la` provides a detailed listing and includes hidden files and directories."

---

## Q7. What does `pwd` do?

**Answer:**

> "`pwd` stands for Print Working Directory. It displays the absolute path of the directory I am currently working in."

---

## Q8. What does `cd ..` do?

**Answer:**

> "`cd ..` moves me from the current directory to its parent directory."

---

## Q9. What does `mkdir` do?

**Answer:**

> "`mkdir` stands for Make Directory and is used to create directories."

---

## Q10. What does `touch` do?

**Answer:**

> "`touch` can create a new empty file and can also update a file's timestamps."

---

# 43. Interview Questions — Intermediate

## Q11. Why is Linux commonly used in DevOps?

**Answer:**

> "Linux is widely used in DevOps because it is stable, open source, automation-friendly and provides powerful command-line tools. Many cloud servers, containers, CI/CD systems and DevOps tools run on Linux."

---

## Q12. Linux vs Unix?

**Answer:**

> "Unix is a family of operating systems that predates Linux. Linux is an open-source Unix-like kernel created by Linus Torvalds. Linux distributions provide complete operating systems built around that kernel."

---

## Q13. What is `sudo`?

**Answer:**

> "`sudo` allows an authorized user to execute a command with elevated privileges. It is commonly required for administrative operations such as installing packages."

---

## Q14. What is `apt`?

**Answer:**

> "APT is a package-management system commonly used on Debian-based Linux distributions such as Ubuntu. It can be used to update package information, install packages and upgrade software."

---

## Q15. What is the difference between `apt update` and `apt upgrade`?

**Answer:**

> "`apt update` refreshes the package index, while `apt upgrade` installs available upgrades for installed packages."

---

# 44. Scenario-Based Interview Questions

## Q16. You log into a Linux server and don't know where you are. What command do you use?

**Answer:**

```bash
pwd
```

---

## Q17. You cannot see a hidden configuration file. What would you do?

**Answer:**

```bash
ls -la
```

Because Linux hidden files commonly start with `.`.

---

## Q18. You need to create a directory called `project`. What command do you use?

**Answer:**

```bash
mkdir project
```

---

## Q19. You need to install Git on Ubuntu. What would you do?

**Answer:**

```bash
sudo apt update
sudo apt install git -y
```

Then verify:

```bash
git --version
```

---

## Q20. You need to connect to a remote Linux server. Which protocol would you commonly use?

**Answer:**

> "I would commonly use SSH, which provides secure remote access. Its default port is 22."

---

# 45. Common Beginner Mistakes

### Mistake 1

Thinking Linux = Ubuntu.

**Correct:**

Ubuntu is a Linux distribution.

---

### Mistake 2

Thinking Linux itself is only a command line.

**Correct:**

Linux is a kernel; Linux distributions can provide graphical environments as well as command-line interfaces.

---

### Mistake 3

Calling multitasking "batch processing."

**Correct:**

Multiple processes/programs running concurrently is multitasking.

---

### Mistake 4

Using `ls` and expecting hidden files.

Use:

```bash
ls -la
```

---

### Mistake 5

Confusing `/` and `/root`.

```text
/       → filesystem root
/root   → root user's home directory
```

---

### Mistake 6

Thinking `touch` is only for creating files.

It can also update file timestamps.

---

# 46. Day 1 Mental Model

Remember Linux using this picture:

```text
                  LINUX
                    |
        +-----------+-----------+
        |                       |
      Kernel                User Space
        |                       |
   +----+----+          +-------+-------+
   |    |    |          |       |       |
 CPU  RAM  Devices     Shell  Python   Git
   |
   ▼
Processes
```

---

# 47. DevOps Connection

Today's commands may look simple, but these are the foundations of real DevOps work.

For example, later you may receive an alert:

> "Production server disk usage is 95%."

You may SSH into the server and use Linux commands to investigate.

Or:

> "Deploy Python application to EC2."

You may:

```text
SSH
 ↓
Linux
 ↓
Git
 ↓
Python
 ↓
Dependencies
 ↓
Application
 ↓
Service
 ↓
Monitoring
```

So don't underestimate these basic commands.

---

# 48. Quick Revision — 5 Minutes

Before moving to Day 2, answer these without looking at the notes:

1. What is Linux?
2. Who created Linux?
3. When was Linux created?
4. What is a kernel?
5. What is a Linux distribution?
6. Give three Linux distributions.
7. What is Ubuntu?
8. What is RHEL?
9. What is WSL?
10. What does `pwd` do?
11. What does `ls` do?
12. Why use `ls -la`?
13. What does `mkdir` do?
14. What does `touch` do?
15. What does `cd ..` do?
16. What does `cat` do?
17. What is `sudo`?
18. What is `apt`?
19. What is SSH?
20. What is the default SSH port?
21. What are HTTP and HTTPS ports?
22. Why is Linux important in DevOps?

If you can answer these **without reading**, your Day 1 foundation is good.

---

# 49. Day 1 Key Takeaways

```text
Linux
 │
 ├── Open Source
 ├── Multi-user
 ├── Multitasking
 ├── Unix-like
 │
 ├── Kernel
 │
 ├── Distributions
 │    ├── Ubuntu
 │    ├── Debian
 │    ├── RHEL
 │    └── Amazon Linux
 │
 ├── CLI
 │    ├── pwd
 │    ├── ls
 │    ├── cd
 │    ├── mkdir
 │    ├── touch
 │    └── cat
 │
 ├── Package Management
 │    └── apt
 │
 ├── Remote Access
 │    └── SSH : 22
 │
 └── DevOps
      ├── Cloud
      ├── Servers
      ├── Docker
      ├── Kubernetes
      └── CI/CD
```

---

# 50. Day 1 Success Criteria

Before calling Day 1 complete, I should be able to:

- [ ] Open WSL
- [ ] Identify my Ubuntu version
- [ ] Navigate directories
- [ ] Create directories
- [ ] Create files
- [ ] Create hidden files/directories
- [ ] Read files
- [ ] Edit files using Nano
- [ ] Install packages with APT
- [ ] Install Python
- [ ] Run a Python program
- [ ] Install Git
- [ ] Explain Linux and the kernel
- [ ] Explain Linux distributions
- [ ] Explain SSH and port 22
- [ ] Explain why Linux is important for DevOps

---

##
