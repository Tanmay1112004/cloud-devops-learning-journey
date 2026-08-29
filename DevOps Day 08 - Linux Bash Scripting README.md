# 🐧 Day 08 — Linux Bash Scripting

> **Course:** Cloud Computing & DevOps  
> **Topic:** Linux Shell & Bash Scripting  
> **Goal:** Learn Bash fundamentals and write scripts for DevOps automation.

---

# 1. What is a Shell?

A **shell** is a command-line interface/interpreter that allows a user to communicate with the operating system.

In Linux, the shell acts as an interface between the **user and the Linux kernel**.

### Basic flow

```text
+---------+
|  User   |
+----+----+
     |
     | Commands
     v
+----+----+
|  Shell  |
|  Bash   |
+----+----+
     |
     | System calls
     v
+----+----+
| Kernel  |
+----+----+
     |
     v
+---------+
| Hardware|
+---------+
```

### Example

When you execute:

```bash
ls
```

the shell interprets the command and requests the kernel to perform the required operation.

---

# 2. What is Bash?

**Bash** stands for:

> **Bourne Again SHell**

Bash is one of the most widely used shells in Linux.

It is both:

1. A **command interpreter**
2. A **scripting language**

Bash is extremely important in DevOps because it is commonly used for:

- Server administration
- Automation
- Deployment scripts
- Backup scripts
- Monitoring
- CI/CD pipelines
- Application setup
- Infrastructure automation

---

# 3. Shell vs Bash

| Shell | Bash |
|---|---|
| General concept/program | Specific shell |
| Provides command interpretation | One implementation of a shell |
| Examples: sh, bash, zsh, fish | Bourne Again Shell |
| Interface between user and OS | A particular command interpreter |

Think of it like:

```text
Shell = Category
Bash  = One member of that category
```

---

# 4. Check Your Current Shell

```bash
echo $SHELL
```

Example output:

```text
/bin/bash
```

This means the user's login shell is Bash.

Another useful command:

```bash
ps -p $$
```

Here:

- `ps` → displays process information
- `-p` → select a process by PID
- `$$` → PID of the current shell

---

# 5. List Available Shells

Linux maintains a list of commonly available login shells in:

```text
/etc/shells
```

Check it using:

```bash
cat /etc/shells
```

Example:

```text
/bin/sh
/bin/bash
/bin/zsh
/bin/fish
```

The exact list depends on the Linux distribution and installed software.

---

# 6. Common Linux Shells

### Bash

```text
/bin/bash
```

Popular general-purpose Linux shell.

### sh

```text
/bin/sh
```

Traditional Bourne shell interface; on many modern Linux systems, `/bin/sh` may point to another shell implementation.

### Zsh

```text
/bin/zsh
```

Feature-rich interactive shell.

### Fish

```text
/usr/bin/fish
```

Friendly Interactive SHell, designed for an improved interactive experience.

---

# 7. Interactive vs Non-Interactive Shell

## Interactive Shell

A shell where the user directly enters commands.

Example:

```bash
ls
cd /var/log
pwd
```

## Non-Interactive Shell

A shell that executes commands from a script or automated process.

Example:

```bash
./script.sh
```

This distinction is important in DevOps because many automation systems execute scripts non-interactively.

---

# 8. Package Management: yum vs dnf vs apt

Your original notes contain:

```bash
sudo yum apt list --upgradable
```

This is incorrect.

These commands belong to different package-management systems.

### Debian/Ubuntu

Uses `apt`:

```bash
sudo apt update
sudo apt upgrade
```

To view upgradeable packages:

```bash
apt list --upgradable
```

### RHEL/CentOS/Amazon Linux families

Commonly use:

```bash
dnf
```

or on older systems:

```bash
yum
```

Examples:

```bash
sudo dnf update -y
```

or:

```bash
sudo yum update -y
```

### Important

Do **not** blindly mix:

```text
apt
yum
dnf
```

Use the package manager supported by your Linux distribution.

---

# 9. Installing PowerShell on Linux

PowerShell is Microsoft's cross-platform shell.

The executable is:

```bash
pwsh
```

After installation:

```bash
pwsh
```

You can verify the current PowerShell version with:

```powershell
$PSVersionTable
```

> **Note:** Installation commands depend on the Linux distribution and PowerShell version. For real projects, prefer Microsoft's current official installation instructions rather than relying on an old version-specific RPM URL from classroom notes.

---

# 10. Fish Shell

Fish means:

> **Friendly Interactive SHell**

It is designed primarily for interactive command-line use and provides features such as:

- Autosuggestions
- Syntax highlighting
- Convenient command completion

Installation depends on your Linux distribution.

For example, on a distribution using `dnf`:

```bash
sudo dnf install fish -y
```

On an older `yum`-based system:

```bash
sudo yum install fish -y
```

Start Fish:

```bash
fish
```

Return to Bash:

```bash
bash
```

Check the configured login shell:

```bash
echo $SHELL
```

> **Important:** Starting `fish` from Bash creates a new shell process. It does not necessarily change your user's default login shell.

---

# 11. Manual Pages

Linux provides documentation through the `man` command.

Example:

```bash
man fish
```

For Bash:

```bash
man bash
```

For PowerShell:

```bash
man pwsh
```

However, `man pwsh` may not provide useful documentation depending on the installation. PowerShell also has its own help system:

```powershell
Get-Help
```

Example:

```powershell
Get-Help Get-Process
```

---

# 12. What is Bash Scripting?

A **Bash script** is a text file containing a sequence of Bash commands that can be executed automatically.

Instead of repeatedly typing:

```bash
mkdir backup
cd backup
touch file.txt
echo "Backup started"
```

we can put the commands into a script and execute it once.

This is the foundation of automation.

---

# 13. Creating Your First Script

Create a file:

```bash
touch script.sh
```

Open it:

```bash
nano script.sh
```

Write:

```bash
#!/bin/bash

echo "Hello from Bash script"
```

Save the file.

View it:

```bash
cat script.sh
```

---

# 14. Shebang

The first line:

```bash
#!/bin/bash
```

is called the **shebang**.

It tells the operating system which interpreter should be used to execute the script when the script is run directly.

For example:

```bash
#!/bin/bash
```

means:

> Execute this script using Bash.

Another common form is:

```bash
#!/usr/bin/env bash
```

This searches for `bash` using the user's environment.

### Interview Question

**Q. What is a shebang?**

A shebang is the first line of a script beginning with `#!`. It specifies the interpreter that should execute the script when it is invoked directly.

---

# 15. Executing a Bash Script

First give execute permission:

```bash
chmod +x script.sh
```

Then:

```bash
./script.sh
```

You can also explicitly invoke Bash:

```bash
bash script.sh
```

### Difference

```bash
./script.sh
```

requires executable permission and uses the script's interpreter declaration when applicable.

```bash
bash script.sh
```

explicitly asks Bash to interpret the file.

---

# 16. Bash Variables

Variables store values.

Example:

```bash
country="India"
```

Print it:

```bash
echo "$country"
```

### Important Bash Rule

Do NOT put spaces around `=`.

Correct:

```bash
name="Tanmay"
```

Incorrect:

```bash
name = "Tanmay"
```

Bash would interpret the latter incorrectly.

---

# 17. Variable Expansion

Use `$` to access the value of a variable:

```bash
name="Tanmay"

echo "$name"
```

Output:

```text
Tanmay
```

You can also use:

```bash
echo "Hello $name"
```

---

# 18. Environment Variables

Linux provides many environment variables.

Examples:

```bash
echo "$HOME"
```

Shows the user's home directory.

```bash
echo "$PATH"
```

Shows directories searched when executing commands.

Other useful variables:

```bash
echo "$USER"
echo "$PWD"
echo "$SHELL"
```

---

# 19. What is PATH?

`PATH` is an environment variable containing directories where the shell searches for executable commands.

Example:

```bash
echo "$PATH"
```

Possible output:

```text
/usr/local/bin:/usr/bin:/bin
```

When you type:

```bash
python
```

the shell searches directories in `PATH` to find the executable.

This concept is extremely important for DevOps.

---

# 20. Exporting Variables

A normal shell variable:

```bash
name="Tanmay"
```

is available to the current shell.

To make it available to child processes:

```bash
export name="Tanmay"
```

Example:

```bash
export APP_ENV="production"
```

Then child processes can access it.

---

# 21. Taking User Input

Use:

```bash
read
```

Example:

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Hello $name"
```

Run:

```bash
./script.sh
```

Example:

```text
Enter your name: Tanmay
Hello Tanmay
```

---

# 22. Hidden Input

For sensitive input such as a password:

```bash
read -s password
```

The `-s` option hides the typed characters.

For example:

```bash
read -s -p "Enter password: " password
echo
```

> In production automation, prefer secure secret-management systems instead of storing passwords directly in scripts.

---

# 23. Arithmetic Operations

Bash supports arithmetic using:

```bash
$(( ))
```

Example:

```bash
a=10
b=5

echo $((a + b))
```

Output:

```text
15
```

### Basic operations

```bash
echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))
echo $((a % b))
```

`%` gives the remainder.

---

# 24. Arithmetic Example

```bash
#!/bin/bash

a=10
b=12

echo "Addition: $((a + b))"
echo "Subtraction: $((a - b))"
echo "Multiplication: $((a * b))"
echo "Division: $((a / b))"
```

Output:

```text
Addition: 22
Subtraction: -2
Multiplication: 120
Division: 0
```

Notice that Bash integer arithmetic performs integer division here.

---

# 25. Conditional Statements

Basic structure:

```bash
if [ condition ]; then
    commands
fi
```

Example:

```bash
#!/bin/bash

a=10
b=5

if [ "$a" -gt "$b" ]; then
    echo "a is greater"
fi
```

---

# 26. Important Numeric Comparison Operators

| Operator | Meaning |
|---|---|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |
| `-lt` | Less than |
| `-le` | Less than or equal |

Example:

```bash
if [ "$a" -gt "$b" ]; then
    echo "a is greater"
fi
```

---

# 27. If-Else

```bash
#!/bin/bash

read -p "Enter a number: " num

if [ "$num" -gt 10 ]; then
    echo "Greater than 10"
else
    echo "10 or less"
fi
```

---

# 28. Logical Operators

Example:

```bash
if [ "$a" -gt 5 ] && [ "$b" -lt 10 ]; then
    echo "Condition is true"
fi
```

Common logical operators:

```text
&&   AND
||   OR
!    NOT
```

Bash also supports compound conditions with `[[ ]]`, which is generally preferable for many Bash-specific scripts.

Example:

```bash
if [[ "$a" -gt 5 && "$b" -lt 10 ]]; then
    echo "Condition is true"
fi
```

---

# 29. For Loop

A `for` loop repeats commands for each item in a list.

```bash
#!/bin/bash

for i in {1..5}
do
    echo "Iteration $i"
done
```

Output:

```text
Iteration 1
Iteration 2
Iteration 3
Iteration 4
Iteration 5
```

---

# 30. While Loop

Your classroom notes called this a "do while" loop.

Strictly speaking, Bash does **not** have a native `do...while` construct.

This is a **while loop**:

```bash
#!/bin/bash

count=1

while [ "$count" -le 5 ]
do
    echo "Count: $count"
    ((count++))
done
```

Output:

```text
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

---

# 31. Functions

Functions allow us to group reusable commands.

Example:

```bash
#!/bin/bash

greet() {
    echo "Hello, $1"
}

greet "Tanmay"
```

Output:

```text
Hello, Tanmay
```

Here:

```text
$1
```

represents the first argument passed to the function.

---

# 32. Command-Line Arguments

A Bash script can receive arguments.

Example:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
```

Run:

```bash
./script.sh hello world
```

Output:

```text
Script name: ./script.sh
First argument: hello
Second argument: world
```

Important special variables:

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$?` | Exit status of previous command |
| `$$` | PID of current shell |

---

# 33. Arrays

Bash supports indexed arrays.

```bash
#!/bin/bash

fruits=("Apple" "Banana" "Cherry")

echo "${fruits[0]}"
echo "${fruits[1]}"
echo "${fruits[2]}"
```

Print all elements:

```bash
echo "${fruits[@]}"
```

Number of elements:

```bash
echo "${#fruits[@]}"
```

> Always pay attention to the braces: `${fruits[@]}`.

---

# 34. Reading a File Line by Line

Suppose:

```text
file.txt
```

contains:

```text
hello
welcome
DevOps
```

Script:

```bash
#!/bin/bash

while IFS= read -r line
do
    echo "$line"
done < file.txt
```

This is a safer/general-purpose pattern than simply:

```bash
while read line
```

because `IFS=` and `-r` help preserve whitespace and backslashes.

---

# 35. Case Statement

`case` is useful when a script has multiple possible choices.

Example:

```bash
#!/bin/bash

echo "Enter Number:"
read -r num

case "$num" in
    1)
        echo "One"
        ;;
    2)
        echo "Two"
        ;;
    *)
        echo "Invalid"
        ;;
esac
```

---

# 36. Practical Menu Script

```bash
#!/bin/bash

echo "Enter a Choice:"
read -r choice

case "$choice" in
    1)
        echo "Creating folder..."
        mkdir -p new
        ;;
    2)
        echo "Creating file..."
        touch a.txt
        ;;
    3)
        echo "Listing files and folders..."
        ls
        ;;
    *)
        echo "Invalid choice"
        ;;
esac
```

This demonstrates how Bash can automate system operations.

---

# 37. File and Directory Commands

Create file:

```bash
touch file.txt
```

Create directory:

```bash
mkdir mydir
```

List files:

```bash
ls
```

Detailed listing:

```bash
ls -la
```

Show hidden files:

```bash
ls -a
```

Display file contents:

```bash
cat file.txt
```

Directory tree:

```bash
tree
```

---

# 38. Disk Usage

The `df` command reports filesystem disk-space usage.

```bash
df
```

Human-readable format:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/xvda1       20G   12G    8G  60%
```

Important:

```text
Size = Total filesystem size
Used = Used space
Avail = Available space
Use% = Percentage used
```

---

# 39. Practical DevOps Example — Disk Usage Monitor

One of the most useful applications of Bash scripting is system monitoring.

```bash
#!/bin/bash

THRESHOLD=80

usage=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ "$usage" -gt "$THRESHOLD" ]; then
    echo "Disk usage is high: $usage%"
else
    echo "Disk usage is normal: $usage%"
fi
```

### What happens here?

```text
df -h /
    ↓
Get disk usage
    ↓
awk
    ↓
Extract Use%
    ↓
sed
    ↓
Remove %
    ↓
Store value in usage
    ↓
Compare with threshold
    ↓
Generate alert
```

This is a simple example of **DevOps automation**.

---

# 40. Understanding the Disk Monitoring Command

### `df -h /`

Shows human-readable disk usage for the root filesystem.

### `awk`

Used to process structured text.

```bash
awk 'NR==2 {print $5}'
```

selects the second line and fifth field.

### `sed`

Used for text transformation.

```bash
sed 's/%//'
```

removes `%`.

### Command substitution

```bash
usage=$(command)
```

runs the command and stores its output in the variable.

This is preferred over old-style backticks:

```bash
usage=`command`
```

---

# 41. Backup Script

A simple backup example:

```bash
#!/bin/bash

src="/etc"
dest="/tmp/backup_$(date +%F_%H-%M-%S).tar.gz"

tar -czf "$dest" "$src"

echo "Backup created: $dest"
```

### Important

Never casually run destructive or privileged backup/restore commands on a production machine without understanding exactly what they will read/write.

---

# 42. Simple Calculator

```bash
#!/bin/bash

read -p "Enter a: " a
read -p "Enter b: " b
read -p "Operator (+ - * /): " op

case "$op" in
    +)
        echo $((a + b))
        ;;
    -)
        echo $((a - b))
        ;;
    \*)
        echo $((a * b))
        ;;
    /)
        if [ "$b" -eq 0 ]; then
            echo "Cannot divide by zero"
        else
            echo $((a / b))
        fi
        ;;
    *)
        echo "Invalid operator"
        ;;
esac
```

---

# 43. Bash Debugging

When a script doesn't behave as expected, debugging is essential.

## Check syntax

```bash
bash -n script.sh
```

This checks syntax without executing the script.

## Debug execution

```bash
bash -x script.sh
```

This prints commands as Bash executes them.

This is very useful in DevOps troubleshooting.

---

# 44. Exit Status

Every command normally returns an exit status.

Convention:

```text
0     = Success
non-zero = Failure/error
```

Check the previous command's status:

```bash
echo $?
```

Example:

```bash
ls
echo $?
```

If `ls` succeeds, you will normally see:

```text
0
```

This concept is extremely important in:

- Shell scripts
- Jenkins
- CI/CD
- Docker
- Kubernetes
- Automation

---

# 45. Important Bash Safety Practices

### Quote variables

Prefer:

```bash
echo "$name"
```

instead of:

```bash
echo $name
```

Quoting helps prevent unwanted word splitting and glob expansion.

### Use `[[ ]]` for Bash conditions

Example:

```bash
if [[ "$name" == "Tanmay" ]]; then
    echo "Hello"
fi
```

### Check input

Don't assume users always provide valid input.

### Check command failures

For important automation, handle errors explicitly.

### Avoid hard-coded secrets

Never put:

```bash
PASSWORD="mypassword123"
```

into a GitHub repository.

Use environment variables or proper secret-management solutions.

---

# 46. Bash in DevOps

Bash is important because DevOps engineers frequently automate repetitive tasks.

Common examples:

```text
Server Setup
     ↓
Install Packages
     ↓
Configure Application
     ↓
Create Directories
     ↓
Set Permissions
     ↓
Start Services
     ↓
Check Status
```

A Bash script can automate the entire process.

---

# 47. Real-World DevOps Uses

Bash scripting can be used for:

### Server administration

```bash
systemctl status nginx
```

### Log processing

```bash
grep "ERROR" application.log
```

### Disk monitoring

```bash
df -h
```

### Backup

```bash
tar -czf backup.tar.gz /data
```

### Deployment

```text
Pull code
   ↓
Install dependencies
   ↓
Build application
   ↓
Restart service
```

### CI/CD

Jenkins, GitHub Actions and other automation systems can execute shell commands.

---

# 48. Shell → Bash → DevOps Relationship

```text
Linux
  │
  ├── Shell
  │     │
  │     ├── Bash
  │     ├── Zsh
  │     ├── Fish
  │     └── Other shells
  │
  └── Bash Scripting
          │
          ├── Automation
          ├── Monitoring
          ├── Deployment
          ├── Backup
          └── CI/CD
```

---

# 🎤 49. Interview Questions

## ⭐ Basic

### Q1. What is a shell?

A shell is a command interpreter that provides an interface between the user and the operating system.

### Q2. What is Bash?

Bash stands for Bourne Again SHell and is a widely used Unix/Linux shell and scripting language.

### Q3. What is the difference between Shell and Bash?

Shell is the general concept/category of command interpreters. Bash is one specific shell implementation.

### Q4. What is a shebang?

A shebang begins with `#!` and specifies the interpreter used to execute a script directly.

Example:

```bash
#!/bin/bash
```

### Q5. How do you execute a Bash script?

```bash
chmod +x script.sh
./script.sh
```

or:

```bash
bash script.sh
```

---

# 🔥 50. Intermediate Interview Questions

### Q6. What is the difference between `$1` and `$@`?

`$1` represents the first positional argument, while `$@` represents all positional arguments.

### Q7. What does `$?` represent?

It contains the exit status of the most recently executed command.

### Q8. What does `$PATH` do?

It contains directories that the shell searches for executable commands.

### Q9. What is `chmod +x`?

It adds execute permission to a file.

### Q10. What is the difference between `=` and `-eq`?

`=` is commonly used for string comparison in test expressions, while `-eq` performs numeric equality comparison.

### Q11. What is `$(command)`?

It is command substitution. The command is executed and its output can be assigned to a variable or used as part of another command.

Example:

```bash
today=$(date)
```

### Q12. How do you debug a Bash script?

```bash
bash -x script.sh
```

### Q13. How do you check Bash syntax without running the script?

```bash
bash -n script.sh
```

### Q14. What is an environment variable?

A variable made available to a process and, when exported, inherited by child processes.

### Q15. Why is Bash important in DevOps?

Because Bash can automate server administration, deployment, monitoring, backups, configuration and CI/CD tasks.

---

# 💼 51. Scenario-Based Interview Questions

### Q1. Your server disk usage reaches 90%. What can you do?

A Bash monitoring script can periodically check `df` output and trigger an alert when usage crosses a predefined threshold.

### Q2. You need to execute the same 10 server commands every morning. What would you do?

Create a Bash script containing the commands and automate its execution using a scheduler such as cron.

### Q3. Your Jenkins pipeline fails while executing a shell command. What would you investigate?

Check:

- Command output
- Exit status
- File permissions
- Environment variables
- PATH
- User permissions
- Logs
- Shell/interpreter being used

### Q4. A script works manually but fails in Jenkins. Why?

Possible causes include differences in:

- User account
- Working directory
- PATH
- Environment variables
- Permissions
- Shell
- Available dependencies

This is a very common real-world DevOps troubleshooting problem.

---

# 🧪 52. Hands-On Practice

Complete these without copying the solution immediately.

### Task 1 — User Greeting

Create:

```text
greet.sh
```

Ask for:

```text
Name
Age
City
```

Print:

```text
Hello Tanmay
You are 21 years old
You live in Latur
```

---

### Task 2 — Number Checker

Take a number from the user and determine whether it is:

```text
Positive
Negative
Zero
```

---

### Task 3 — Even/Odd

Take a number and determine whether it is even or odd.

Hint:

```bash
%
```

---

### Task 4 — Multiplication Table

Ask the user for a number and print its table from 1 to 10.

---

### Task 5 — File Checker

Ask the user for a filename and determine whether the file exists.

---

### Task 6 — Disk Monitor

Create a script that:

- Gets root filesystem usage
- Compares it with a threshold
- Prints a warning when usage is high

---

### Task 7 — Menu System

Create a menu:

```text
1. Show current directory
2. List files
3. Show disk usage
4. Show current user
5. Exit
```

Implement it using `case`.

---

# 🚀 53. Mini Project — Linux System Information Script

Build a script called:

```text
system-info.sh
```

It should display:

```text
=========================
 Linux System Information
=========================

Hostname:
Current User:
Current Directory:
Current Date:
Kernel Version:
Uptime:
Memory Usage:
Disk Usage:
```

Useful commands:

```bash
hostname
whoami
pwd
date
uname -r
uptime
free -h
df -h
```

This is a good beginner DevOps automation project.

---

# 📌 54. GitHub Structure for Day 08

I recommend putting Day 08 under Linux rather than creating a separate repository.

```text
cloud-computing-devops-learning/
│
├── 01-Linux/
│   │
│   ├── Notes/
│   │   └── Day-08-Bash-Scripting.md
│   │
│   ├── Commands/
│   │   └── Bash-Commands.md
│   │
│   ├── Scripts/
│   │   ├── 01-hello-world.sh
│   │   ├── 02-variables.sh
│   │   ├── 03-user-input.sh
│   │   ├── 04-conditionals.sh
│   │   ├── 05-loops.sh
│   │   ├── 06-functions.sh
│   │   ├── 07-arrays.sh
│   │   ├── 08-file-handling.sh
│   │   └── 09-case-statement.sh
│   │
│   └── Projects/
│       ├── disk-monitor.sh
│       ├── backup-script.sh
│       ├── calculator.sh
│       └── system-info.sh
│
└── README.md
```

---

# ✅ 55. Day 08 Quick Revision

Remember these:

```text
Shell
  → Interface between user and OS

Bash
  → Bourne Again SHell

#!/bin/bash
  → Shebang

chmod +x
  → Add execute permission

./script.sh
  → Execute script

$HOME
  → User home directory

$PATH
  → Command search path

$?
  → Previous command exit status

$1
  → First argument

$@
  → All arguments

$(( ))
  → Arithmetic

if
  → Conditional execution

for
  → Iteration over items

while
  → Repeated execution while condition is true

function
  → Reusable block of commands

case
  → Multiple-choice branching

df -h
  → Human-readable filesystem usage

bash -x
  → Debug execution

bash -n
  → Syntax check
```

---

# 🎯 Day 08 Interview Priority

### 🔥 MUST KNOW

- What is Shell?
- What is Bash?
- Bash vs Shell
- What is shebang?
- How to execute a Bash script?
- Variables
- Environment variables
- `$PATH`
- `$HOME`
- `if/else`
- Loops
- Functions
- Arrays
- `case`
- Command-line arguments
- `$?`
- `chmod +x`
- `df -h`

### ⭐ NEXT LEVEL

- `[[ ]]` vs `[ ]`
- Exit codes
- `bash -x`
- `bash -n`
- Command substitution
- Process vs shell
- Interactive vs non-interactive shell
- Why scripts behave differently in Jenkins
- Bash in CI/CD
- Disk monitoring automation

---

# 🧠 Final Concept

The most important thing from today's class is not memorizing Bash syntax.

Understand this:

```text
Linux Administration
        ↓
Bash Commands
        ↓
Bash Scripting
        ↓
Automation
        ↓
DevOps
        ↓
CI/CD + Cloud + Infrastructure
```

**Bash is one of the fundamental automation skills for a DevOps engineer.**