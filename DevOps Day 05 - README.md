# DevOps Day 05 — Linux File Permissions, File Management, Shell Scripting & Users

## Learning Objectives

- Linux file permissions: `r`, `w`, `x`
- Permission values: `4`, `2`, `1`
- `chmod`
- User, group and others
- File and directory management
- `touch`, `mkdir`, `mv`, `rm`
- Bash brace expansion
- Basic Bash scripting
- Shebang and executable scripts
- Linux users and `/etc/passwd`
- `useradd`, `passwd`, `su`, `whoami`
- `/home` directories
- `tree`

---

## 1. Linux File Permissions

Run:

```bash
ls -la
```

Example:

```text
-rw-r--r-- 1 ec2-user ec2-user 12 Aug 27 a.txt
```

Breakdown:

```text
- rw- r-- r--
│  │   │   │
│  │   │   └── Others
│  │   └────── Group
│  └────────── User / Owner
└───────────── File type
```

### File types

```text
-  → regular file
d  → directory
l  → symbolic link
```

---

## 2. Permission Types

| Permission | Symbol | Value |
|---|---:|---:|
| Read | `r` | 4 |
| Write | `w` | 2 |
| Execute | `x` | 1 |

Remember:

```text
r = 4
w = 2
x = 1
```

### Permission combinations

```text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2     = 6
r-x = 4 + 1     = 5
-wx = 2 + 1     = 3
r-- = 4
-w- = 2
--x = 1
--- = 0
```

### Cheat sheet

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
3 = -wx
2 = -w-
1 = --x
0 = ---
```

---

## 3. Permission Levels

Linux permissions are assigned to:

```text
User   → Owner
Group  → Group members
Others → Everyone else
```

Example:

```text
-rwxr-xr--
```

Means:

```text
User   → rwx
Group  → r-x
Others → r--
```

---

## 4. chmod

`chmod` means **change mode**.

Syntax:

```bash
chmod PERMISSION FILE
```

Example:

```bash
chmod 600 a.txt
```

### Common examples

```bash
chmod 000 a.txt
chmod 400 a.txt
chmod 600 a.txt
chmod 755 a.txt
chmod 777 a.txt
```

`chmod 755` means:

```text
User   → rwx
Group  → r-x
Others → r-x

755 = rwxr-xr-x
```

`chmod 600` means:

```text
User   → rw-
Group  → ---
Others → ---

600 = rw-------
```

`chmod 777` gives read, write and execute permission to user, group and others.

> **Security note:** Avoid `777` unless it is genuinely required. Use the minimum permissions needed.

---

## 5. Practical Permission Exercise

```bash
mkdir project
cd project

touch a.txt
ls -la

chmod 000 a.txt
ls -la

chmod 400 a.txt
ls -la

chmod 600 a.txt
ls -la

chmod 755 a.txt
ls -la

chmod 777 a.txt
ls -la
```

Observe how the permission string changes.

---

## 6. File Management Commands

Create a file:

```bash
touch a.txt
```

Create a directory:

```bash
mkdir new
```

List files:

```bash
ls
ls -la
```

Rename a file:

```bash
mv a.txt b.txt
```

Move a file:

```bash
mv a.txt new/
```

Delete a file:

```bash
rm a.txt
```

Delete a directory and its contents:

```bash
rm -rf new/
```

> **Warning:** `rm -rf` is destructive. Always verify the path before running it.

---

## 7. Multiple Files and Directories

Create multiple files:

```bash
touch a.txt b.txt c.txt d.txt
```

Create multiple directories:

```bash
mkdir a b c d f
```

Delete multiple directories:

```bash
rm -rf a b c d
```

Delete multiple files:

```bash
rm a.txt b.txt c.txt d.txt
```

---

## 8. Bash Brace Expansion

Create alphabetically named files:

```bash
touch {a..z}.txt
```

Create numbered files:

```bash
touch {1..100}.txt
```

Create alphabetically named directories:

```bash
mkdir {a..z}
```

Delete them:

```bash
rm -rf {a..z}
```

Brace expansion is useful for repetitive tasks and automation.

---

# 9. Bash Shell and Shell Scripting

Check available shells:

```bash
cat /etc/shells
```

Bash is commonly:

```text
/bin/bash
```

A **shell script** is a file containing shell commands that can be executed as a sequence.

This is important in DevOps because repetitive server tasks can be automated.

---

## 10. Create Your First Bash Script

```bash
touch script.sh
nano script.sh
```

Add:

```bash
#!/bin/bash

echo "Hello from DevOps Day 5"
```

The first line is called the **shebang**. It specifies the interpreter used to execute the script.

---

## 11. Make the Script Executable

```bash
chmod +x script.sh
```

Check:

```bash
ls -la
```

You may see:

```text
-rwxr-xr-x
```

Run:

```bash
./script.sh
```

You can also run:

```bash
bash script.sh
```

`./script.sh` requires execute permission, while `bash script.sh` directly invokes Bash.

---

## 12. Script Example — Create Folder and File

```bash
#!/bin/bash

echo "Starting setup..."

mkdir -p new
touch new.txt

echo "Folder and file created successfully."
```

Run:

```bash
chmod +x script.sh
./script.sh
```

Verify:

```bash
ls -la
tree
```

---

## 13. Why Shell Scripting is Important in DevOps

Instead of manually running many commands:

```text
Command 1
Command 2
Command 3
...
Command 20
```

you can automate them:

```bash
./setup.sh
```

Common DevOps uses:

- Server setup
- Software installation
- Backups
- Log processing
- Monitoring
- Health checks
- Deployment
- CI/CD automation
- Repetitive administration

---

# 14. Linux Users

Linux is a **multi-user operating system**.

Examples:

```text
root
ec2-user
tanmay
developer
jenkins
```

Different users can have different:

- Permissions
- Groups
- Home directories
- Processes
- Access rights

---

## 15. `/etc/passwd`

View local account information:

```bash
cat /etc/passwd
```

A simplified entry:

```text
tanmay:x:1001:1001::/home/tanmay:/bin/bash
```

It contains information such as:

```text
Username
UID
GID
Home directory
Login shell
```

### Important

Modern Linux systems do not normally store actual passwords in `/etc/passwd`.

Password hashes are generally stored in:

```text
/etc/shadow
```

and access to that file is restricted.

---

## 16. Create a Linux User

```bash
sudo useradd tanmay
```

Verify:

```bash
cat /etc/passwd
```

---

## 17. Set a Password

```bash
sudo passwd tanmay
```

Enter the password when prompted.

---

## 18. Switch User

```bash
su tanmay
```

Verify:

```bash
whoami
```

Expected:

```text
tanmay
```

Return:

```bash
exit
```

---

## 19. User Home Directory

User home directories are normally under:

```text
/home/
```

For example:

```text
/home/tanmay
```

Navigate:

```bash
cd /home/tanmay
pwd
```

---

## 20. Root User and sudo

The root user has administrative privileges.

A normal user prompt commonly ends with:

```text
$
```

A root prompt commonly ends with:

```text
#
```

`sudo` allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo yum update -y
```

Use elevated privileges only when required.

---

# 21. tree Command

Install:

```bash
sudo yum install tree -y
```

Run:

```bash
tree
```

Example:

```text
project
├── a.txt
├── b.txt
└── new
    └── script.sh
```

---

# 22. Day 5 Interview Questions

### Q1. What are Linux file permissions?

> Linux file permissions control who can read, write or execute a file or directory.

### Q2. What are the three basic permissions?

> Read, write and execute, represented by `r`, `w` and `x`.

### Q3. What are their values?

> Read is 4, write is 2 and execute is 1.

### Q4. What does `chmod 755` mean?

> The owner has read, write and execute permissions, while the group and others have read and execute permissions.

```text
755 = rwxr-xr-x
```

### Q5. What does `chmod 600` mean?

> The owner has read and write permissions, while group and others have no permissions.

### Q6. What is chmod?

> `chmod` means change mode. It is used to modify file and directory permissions.

### Q7. Difference between `rm` and `rm -rf`?

> `rm` is commonly used to remove files. `rm -rf` recursively removes directories and their contents, so it must be used carefully.

### Q8. What is a shell script?

> A shell script is a file containing shell commands that can be executed as a sequence to automate tasks.

### Q9. What is a shebang?

> A shebang is the first line of a script that specifies its interpreter, such as `#!/bin/bash`.

### Q10. Why use `chmod +x`?

> It adds execute permission to a file so the script can be executed directly.

### Q11. What is `/etc/passwd`?

> `/etc/passwd` contains Linux user account information such as usernames, UIDs, GIDs, home directories and login shells.

### Q12. How do you create a Linux user?

```bash
sudo useradd tanmay
```

### Q13. How do you set a user's password?

```bash
sudo passwd tanmay
```

### Q14. How do you switch to another user?

```bash
su tanmay
```

### Q15. Difference between `$` and `#`?

> `$` commonly represents a normal user's shell prompt, while `#` commonly represents a root shell prompt.

---

# 23. Scenario-Based Interview Question

**Interviewer:** A script works with `bash script.sh`, but `./script.sh` gives `Permission denied`. What will you check?

### Strong Answer

> "First, I would check the permissions using `ls -l script.sh`. If execute permission is missing, I would run `chmod +x script.sh`. Then I would verify the shebang, such as `#!/bin/bash`, and run the script again using `./script.sh`."

---

# 24. Practical Assignment

Create:

```text
day5-project/
├── files/
│   ├── a.txt
│   ├── b.txt
│   └── c.txt
├── scripts/
│   └── setup.sh
└── README.md
```

Commands:

```bash
mkdir day5-project
cd day5-project

mkdir files scripts
touch files/a.txt files/b.txt files/c.txt

nano scripts/setup.sh
```

Add:

```bash
#!/bin/bash

echo "Starting setup..."

mkdir -p test-folder
touch test.txt

echo "Folder and file created successfully."
```

Then:

```bash
chmod +x scripts/setup.sh
./scripts/setup.sh

ls -la
tree
```

---

# 25. Day 5 Cheat Sheet

```bash
# Files
touch a.txt
cat a.txt
nano a.txt

# Directories
mkdir new
cd new
cd ..
pwd

# Listing
ls
ls -la
tree

# Move / Rename
mv a.txt b.txt
mv a.txt new/

# Delete
rm a.txt
rm -rf new/

# Permissions
chmod 000 a.txt
chmod 400 a.txt
chmod 600 a.txt
chmod 755 a.txt
chmod 777 a.txt

# Permission values
r = 4
w = 2
x = 1

# Multiple files
touch {a..z}.txt
touch {1..100}.txt

# Multiple directories
mkdir {a..z}

# Shell scripting
touch script.sh
nano script.sh
chmod +x script.sh
./script.sh

# Users
cat /etc/passwd
sudo useradd tanmay
sudo passwd tanmay
su tanmay
whoami
exit

# Home
cd /home/tanmay
```

---

# 26. Day 5 Success Checklist

- [ ] Understand Linux permissions
- [ ] Understand `r`, `w`, `x`
- [ ] Remember `4`, `2`, `1`
- [ ] Understand `chmod`
- [ ] Understand `chmod 000`
- [ ] Understand `chmod 400`
- [ ] Understand `chmod 600`
- [ ] Understand `chmod 755`
- [ ] Understand why `777` should usually be avoided
- [ ] Understand file vs directory permissions
- [ ] Use `touch`
- [ ] Use `mkdir`
- [ ] Use `mv`
- [ ] Use `rm`
- [ ] Use brace expansion
- [ ] Create and execute Bash scripts
- [ ] Understand shebang
- [ ] Use `chmod +x`
- [ ] Understand `/etc/passwd`
- [ ] Create Linux users
- [ ] Set passwords
- [ ] Switch users using `su`
- [ ] Understand `$` vs `#`
- [ ] Understand `/home`
- [ ] Use `tree`

---

# 🔥 Day 5 Interview Summary

If the interviewer asks **"What did you learn in Linux?"**, you can answer:

> "I learned Linux file and directory management, file permissions, users and basic shell scripting. I practiced permissions using `chmod` and understood the `rwx` model with values 4, 2 and 1. I also created and managed Linux users, worked with `/etc/passwd`, switched between users using `su`, and created Bash scripts using a shebang and execute permissions. These concepts are important for server administration and DevOps automation."

---

# DevOps Learning Progress

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
Linux Permissions
+ File Management
+ Bash Scripting
+ User Management
       ↓
Day 06
Next Topic
```
