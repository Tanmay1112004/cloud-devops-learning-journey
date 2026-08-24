# DevOps Day 02 — Linux Tools, Package Management & Programming Runtimes

> **Course:** Cloud Computing & DevOps  
> **Institute:** Ethans Tech Pune  
> **Topic:** Linux Tools, Package Management, Users & Programming Runtimes  
> **Goal:** Install development tools, understand package managers, Linux users/privileges, command history, and how Python, Java and Node.js programs are executed.

---

# 1. Learning Objectives

By the end of Day 2, I should be able to:

- Install development tools on Windows using Chocolatey.
- Understand package managers.
- Understand Linux package management using APT.
- Update and upgrade Ubuntu packages.
- Use the `history` command.
- Understand normal user vs root user.
- Understand `$` and `#` prompts.
- Install Python, Git, Nano, Java and Node.js.
- Check installed software versions.
- Understand Python interpretation.
- Understand Java compilation and execution.
- Understand the basic Node.js runtime.
- Explain package managers in interviews.

---

# 2. Day 2 Big Picture

Today's learning can be divided into four areas:

```text
                    DAY 2
                      |
        +-------------+-------------+
        |             |             |
    Tool Setup    Linux Admin    Programming
        |             |             |
   Chocolatey      APT/Sudo       Python
   Git             Users          Java
   Python          Root           Node.js
   Node.js         History
```

---

# 3. What is a Package Manager?

A **package manager** is a tool that helps us install, update, remove and manage software packages and their dependencies.

Instead of manually:

```text
Download software
       ↓
Find installer
       ↓
Install
       ↓
Configure
       ↓
Manage dependencies
       ↓
Update manually
```

we can use:

```text
Package Manager
       |
       ├── Search
       ├── Install
       ├── Update
       ├── Upgrade
       └── Remove
```

This is extremely important in DevOps because servers frequently need software to be installed automatically.

---

# 4. Why Do We Need Package Managers?

Suppose you need Git, Python and Java on a server.

Without a package manager:

```text
Find Git installer
Download
Install

Find Python installer
Download
Install

Find Java installer
Download
Install
```

With a package manager:

```bash
sudo apt install git python3 default-jdk -y
```

Much easier to automate.

---

# 5. Common Package Managers

Different operating systems/distributions use different package-management tools.

| Operating System / Distribution | Package Manager |
|---|---|
| Ubuntu | APT |
| Debian | APT |
| RHEL | DNF / YUM |
| Fedora | DNF |
| Arch Linux | Pacman |
| Windows | Chocolatey / WinGet |
| macOS | Homebrew |

### Important Interview Point

Don't say:

> "Linux has one package manager."

Different Linux distributions use different package managers.

---

# 6. APT

APT stands for:

**Advanced Package Tool**

It is commonly used on Debian-based distributions such as Ubuntu.

Examples:

```bash
sudo apt update
sudo apt upgrade
sudo apt install git
sudo apt remove git
sudo apt search git
```

---

# 7. APT Commands

## Update package information

```bash
sudo apt update
```

This refreshes the package index.

---

## Upgrade installed packages

```bash
sudo apt upgrade -y
```

This upgrades installed packages when newer versions are available.

---

## Search for a package

```bash
sudo apt search python
```

---

## Install a package

```bash
sudo apt install git -y
```

---

## Remove a package

```bash
sudo apt remove git -y
```

---

# 8. `apt list --upgradable`

This command shows packages that have available upgrades:

```bash
sudo apt list --upgradable
```

Typical workflow:

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
```

Think:

```text
update
   ↓
Check available upgrades
   ↓
upgrade
```

---

# 9. Chocolatey

## What is Chocolatey?

Chocolatey is a package manager for Windows.

It allows software to be installed and managed from the command line.

Official documentation describes Chocolatey as a software-management tool for Windows.

Official site:

[Chocolatey](https://chocolatey.org/?utm_source=chatgpt.com)

---

# 10. Installing Chocolatey

Open:

**PowerShell → Run as Administrator**

Chocolatey's official installation documentation requires an administrative shell for the normal installation flow.

The official PowerShell installation command is:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

After installation, verify:

```powershell
choco --version
```

---

# 11. Installing Git Using Chocolatey

Once Chocolatey is installed:

```powershell
choco install git
```

Chocolatey's `choco install` command installs a package from its configured package source.

Depending on the prompt, you may need to confirm installation. You can also use:

```powershell
choco install git -y
```

---

# 12. Installing Python Using Chocolatey

```powershell
choco install python -y
```

Verify:

```powershell
python --version
```

or:

```powershell
python3 --version
```

---

# 13. Windows vs WSL Package Installation

This is an important concept.

You have **two environments**:

```text
Windows
   |
   └── Chocolatey
         |
         ├── Git
         └── Python

WSL / Ubuntu
   |
   └── APT
         |
         ├── Git
         ├── Python
         ├── Java
         └── Nano
```

These are different environments.

Installing Git with:

```powershell
choco install git
```

doesn't mean you've installed Git inside your Ubuntu WSL environment through APT.

Inside WSL, you can use:

```bash
sudo apt install git -y
```

---

# 14. WSL Package Management

Enter WSL:

```powershell
wsl
```

Then:

```bash
sudo apt update
```

Check available upgrades:

```bash
sudo apt list --upgradable
```

Upgrade packages:

```bash
sudo apt upgrade -y
```

---

# 15. `history`

The `history` command displays previously executed shell commands.

```bash
history
```

Example:

```text
100  pwd
101  ls
102  mkdir project
103  cd project
104  python --version
```

This is extremely useful when learning Linux.

---

# 16. Clearing History

Your notes mention:

```bash
history -c
```

This clears the current shell's in-memory history list.

However, **be careful** with history deletion.

In real production systems, command history can be useful for auditing and troubleshooting.

Also remember that shell history behavior depends on the shell and configuration, so `history -c` should not be treated as a guaranteed secure eraser of every historical record.

### Security Rule

Never rely on deleting shell history to hide credentials.

**Never type passwords, API keys or secrets directly into commands if they could end up in shell history.**

---

# 17. Creating a Project Directory

Example:

```bash
mkdir project
```

Check:

```bash
ls
```

Find your current location:

```bash
pwd
```

---

# 18. Moving Around the Filesystem

Go to the filesystem root:

```bash
cd /
```

Check:

```bash
pwd
```

Expected:

```text
/
```

Return to your home directory:

```bash
cd
```

Check:

```bash
pwd
```

This normally takes you back to your user's home directory.

---

# 19. Installing Tree

Inside Ubuntu:

```bash
sudo apt install tree -y
```

Check:

```bash
tree --version
```

Run:

```bash
tree
```

For hidden files:

```bash
tree -a
```

---

# 20. Installing Python

Install Python 3:

```bash
sudo apt update
sudo apt install python3 -y
```

Check:

```bash
python3 --version
```

If you want `python` to invoke Python 3 on Ubuntu:

```bash
sudo apt install python-is-python3 -y
```

Then:

```bash
python --version
```

---

# 21. Python Execution Model

Python is generally described as an **interpreted language**, although modern Python implementations also compile source code to bytecode internally before execution.

For a beginner-level model:

```text
helloworld.py
      |
      ▼
Python Interpreter
      |
      ▼
Execute Program
      |
      ▼
Output
```

Example:

```python
print("Hello World")
```

Run:

```bash
python helloworld.py
```

---

# 22. Python Practical

Create:

```bash
touch helloworld.py
```

Open:

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
python helloworld.py
```

Expected:

```text
Hello World
```

---

# 23. Root User

Linux has a special administrative user called:

**root**

The root user has very high privileges.

You can switch to a root shell using:

```bash
sudo -i
```

or, depending on the system configuration:

```bash
sudo su
```

You will typically see a prompt ending in:

```text
#
```

Example:

```text
root@ubuntu:~#
```

---

# 24. Normal User vs Root

A common convention is:

```text
Normal user:
$

Root user:
#
```

Example:

```text
tanmay@ubuntu:~$
```

versus:

```text
root@ubuntu:~#
```

### Important

`$` and `#` are **prompt conventions**, not the actual permissions themselves.

---

# 25. Why Root is Dangerous

Root can perform powerful operations such as:

- Modify system configuration
- Install/remove system software
- Modify protected files
- Manage users
- Stop services
- Change permissions
- Delete important system files

For example, running destructive commands as root can damage the system.

Therefore:

> Use the least privilege necessary.

This is called the **principle of least privilege**.

---

# 26. `sudo` vs `su`

### `sudo`

Runs a specific command with elevated privileges.

Example:

```bash
sudo apt update
```

### `su`

Switches to another user, commonly root.

Example:

```bash
sudo su
```

### `sudo -i`

Starts an interactive root login shell:

```bash
sudo -i
```

For everyday administration, using `sudo` for individual commands is often preferable to staying logged in as root.

---

# 27. Installing Nano

```bash
sudo apt install nano -y
```

Check:

```bash
nano --version
```

---

# 28. Java

Java is another important programming language used in enterprise applications and DevOps environments.

Before installing Java, you can search available packages:

```bash
sudo apt search openjdk
```

A common installation option is:

```bash
sudo apt install default-jdk -y
```

Check:

```bash
java --version
```

---

# 29. JDK vs JRE vs JVM

This is important for interviews.

### JVM

**JVM = Java Virtual Machine**

It executes Java bytecode.

### JRE

**JRE = Java Runtime Environment**

Provides the components required to run Java applications, including the JVM and supporting libraries.

### JDK

**JDK = Java Development Kit**

Used for developing Java applications.

It includes tools such as the Java compiler.

Simple model:

```text
JDK
 |
 ├── Development Tools
 │      └── javac
 │
 └── JRE
       |
       └── JVM
             |
             └── Runs Bytecode
```

---

# 30. Java Compilation and Execution

Java follows a different process from Python.

```text
Java Source Code
      |
      | javac
      ▼
Bytecode (.class)
      |
      | JVM
      ▼
Program Execution
```

Example:

```text
HelloWorld.java
       |
       | javac
       ▼
HelloWorld.class
       |
       | java
       ▼
Output
```

---

# 31. Important Java Naming Rule

Your notes contain an important concept.

If you write:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

the filename must be:

```text
HelloWorld.java
```

because the class is declared as:

```java
public class HelloWorld
```

### Correct

```text
HelloWorld.java
        |
        ▼
public class HelloWorld
```

### Incorrect

```text
helloworld.java
        |
        ▼
public class HelloWorld
```

For a public Java class, the source filename must match the public class name exactly, including capitalization.

---

# 32. Java Hello World — Correct Practical

Create:

```bash
touch HelloWorld.java
```

Edit:

```bash
nano HelloWorld.java
```

Write:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Compile:

```bash
javac HelloWorld.java
```

This creates:

```text
HelloWorld.class
```

Run:

```bash
java HelloWorld
```

Expected:

```text
Hello, World!
```

---

# 33. Very Important: `javac` vs `java`

Remember:

```text
javac → compile Java source code

java → run Java application
```

Example:

```bash
javac HelloWorld.java
java HelloWorld
```

Don't write:

```bash
java HelloWorld.java
```

when learning the traditional compile-then-run workflow.

---

# 34. Java vs Python

| Feature | Python | Java |
|---|---|---|
| Source file | `.py` | `.java` |
| Typical execution | Interpreter/runtime | Compile to bytecode, then JVM |
| Compiler command | Not normally used directly | `javac` |
| Run command | `python file.py` | `java ClassName` |
| Intermediate bytecode | Python bytecode internally | `.class` bytecode |
| Runtime | Python interpreter/implementation | JVM |

Simple memory:

```text
Python:

.py
 ↓
Python runtime
 ↓
Output
```

```text
Java:

.java
 ↓
javac
 ↓
.class
 ↓
JVM
 ↓
Output
```

---

# 35. Node.js

Node.js is a JavaScript runtime that allows JavaScript to run outside the browser.

It is commonly used for:

- Backend applications
- APIs
- Web servers
- Automation
- Build tooling
- Developer tools

Node.js provides an environment for executing JavaScript on systems such as Linux.

---

# 36. NVM

## NVM = Node Version Manager

NVM allows you to install and switch between Node.js versions.

This is useful because different projects may require different Node.js versions.

```text
Project A → Node 20
Project B → Node 22
Project C → Node 24
```

NVM helps manage those versions.

The official Node.js download page currently provides installation instructions using NVM and lists Node.js 24.19.0 as the latest LTS release at the time of this note.

---

# 37. Installing Node.js Using NVM

The official Node.js page currently provides an NVM-based Linux installation flow.

Install NVM:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.6/install.sh | bash
```

Load NVM into the current shell:

```bash
\. "$HOME/.nvm/nvm.sh"
```

Install Node.js 24:

```bash
nvm install 24
```

Check:

```bash
node -v
```

Check npm:

```bash
npm -v
```

> **Version note:** Your class notes mention `v24.19.0` and npm `11.17.0`. Those are version-specific outputs and can change over time. Always use the version shown by your current installation rather than expecting an exact version number.

---

# 38. Node.js Architecture — Simple View

```text
JavaScript Code
       |
       ▼
    Node.js
       |
       ▼
 JavaScript Runtime
       |
       ▼
    Operating System
       |
       ▼
     Hardware
```

---

# 39. What is npm?

**npm = Node Package Manager**

It is used to install and manage JavaScript/Node.js packages.

Example:

```bash
npm install
```

A Node.js project commonly contains:

```text
package.json
package-lock.json
node_modules/
```

We'll study these properly when we reach Node.js development.

---

# 40. Three Programming Runtimes — Mental Model

Today's biggest programming concept:

```text
              PROGRAMMING
                   |
       +-----------+-----------+
       |           |           |
     Python       Java      JavaScript
       |           |           |
       ▼           ▼           ▼
 Python        JVM          Node.js
 Runtime
```

---

# 41. Compilation vs Interpretation

This is a common interview topic.

### Compilation

Source code is transformed into another form before execution.

Java example:

```text
.java
 ↓
javac
 ↓
.class bytecode
 ↓
JVM
```

### Interpretation / Runtime Execution

A runtime executes the program through an interpreter/runtime system.

Python beginner model:

```text
.py
 ↓
Python runtime
 ↓
Execution
```

### Important

Modern language implementations are more complicated than this simplified model.

For interviews, explain the basic model first and add details only when asked.

---

# 42. Package Manager Interview Questions

## Q1. What is a package manager?

**Answer:**

> "A package manager is a tool used to install, update, remove and manage software packages and their dependencies."

---

## Q2. What package manager is used by Ubuntu?

**Answer:**

> "Ubuntu commonly uses APT for package management."

---

## Q3. What package manager is commonly associated with RHEL?

**Answer:**

> "Modern RHEL-based systems commonly use DNF. Older systems and commands often used YUM, and `yum` may still exist as a compatibility interface on some systems."

This is better than simply saying:

> "RHEL = yum."

---

## Q4. What is Chocolatey?

**Answer:**

> "Chocolatey is a Windows package manager that allows software to be installed and managed from the command line."

---

## Q5. Why are package managers important in DevOps?

**Answer:**

> "Package managers make software installation, upgrades and dependency management easier to automate. This is especially useful when configuring cloud servers and CI/CD environments."

---

# 43. Linux Administration Interview Questions

## Q6. What is the difference between a normal user and root?

**Answer:**

> "A normal user has limited permissions, while root has highly privileged administrative access to the Linux system."

---

## Q7. What does `sudo` do?

**Answer:**

> "`sudo` allows an authorized user to execute a command with elevated privileges."

---

## Q8. What is the difference between `sudo` and `su`?

**Answer:**

> "`sudo` normally runs a command with elevated privileges, while `su` switches to another user account. For example, `sudo su` can start a root shell."

---

## Q9. What does `$` mean in a Linux prompt?

**Answer:**

> "It commonly indicates a normal user's shell prompt."

---

## Q10. What does `#` mean?

**Answer:**

> "It commonly indicates a root or privileged shell prompt."

---

# 44. Programming Interview Questions

## Q11. How do you run a Python program?

**Answer:**

```bash
python3 helloworld.py
```

or:

```bash
python helloworld.py
```

if `python` is configured to point to Python 3.

---

## Q12. How do you compile a Java program?

**Answer:**

```bash
javac HelloWorld.java
```

---

## Q13. How do you run a compiled Java program?

**Answer:**

```bash
java HelloWorld
```

---

## Q14. What is the difference between `javac` and `java`?

**Answer:**

> "`javac` is the Java compiler used to compile source code into bytecode, while `java` launches the Java application using the JVM."

---

## Q15. Why does the Java filename need to match a public class name?

**Answer:**

> "In Java, when a class is declared public, the source filename must match that public class name exactly, including capitalization."

---

# 45. Scenario-Based Interview Questions

## Q16. You are configuring a new Ubuntu server and need Git and Python. What would you do?

```bash
sudo apt update
sudo apt install git python3 -y
```

Verify:

```bash
git --version
python3 --version
```

---

## Q17. You need to know which commands you previously executed. What do you use?

```bash
history
```

---

## Q18. You need to see which packages can be upgraded.

```bash
sudo apt update
sudo apt list --upgradable
```

---

## Q19. You need to upgrade available packages.

```bash
sudo apt upgrade -y
```

---

## Q20. A Java developer gives you `HelloWorld.java`. What commands do you use?

```bash
javac HelloWorld.java
java HelloWorld
```

---

# 46. Common Mistakes From Day 2

### Mistake 1 — Wrong Java filename

Don't do:

```text
helloworld.java
public class HelloWorld
```

Use:

```text
HelloWorld.java
public class HelloWorld
```

---

### Mistake 2 — Running Java incorrectly

Don't use:

```bash
java HelloWorld.java
```

for the traditional compile/run workflow you're learning.

Use:

```bash
javac HelloWorld.java
java HelloWorld
```

---

### Mistake 3 — Confusing Windows and WSL packages

Windows:

```powershell
choco install git
```

Ubuntu WSL:

```bash
sudo apt install git -y
```

---

### Mistake 4 — Assuming `yum` is the current default everywhere

For modern RHEL-based systems, know:

```text
DNF → modern package manager
YUM → older/common compatibility terminology
```

---

### Mistake 5 — Staying root unnecessarily

Instead of:

```bash
sudo -i
```

and remaining root for everything, prefer normal-user work plus `sudo` when administrative privileges are required.

---

### Mistake 6 — Expecting exact Node.js versions forever

Versions change.

Instead of memorizing:

```text
v24.19.0
```

understand:

```text
nvm
 ↓
install Node version
 ↓
node -v
 ↓
npm -v
```

---

# 47. Hands-On Lab — Day 2

Do this yourself in WSL.

## Lab 1 — System Update

```bash
sudo apt update
sudo apt list --upgradable
sudo apt upgrade -y
```

---

## Lab 2 — Install Tools

```bash
sudo apt install tree nano git python3 -y
```

Verify:

```bash
tree --version
nano --version
git --version
python3 --version
```

---

## Lab 3 — History

Run:

```bash
pwd
ls
mkdir devops-day2
cd devops-day2
touch notes.txt
ls
history
```

Observe how your commands appear in history.

---

## Lab 4 — Python

Create:

```bash
nano hello.py
```

Write:

```python
print("I am learning DevOps")
print("Today I learned Linux tools")
```

Run:

```bash
python3 hello.py
```

---

## Lab 5 — Java

Create:

```bash
nano HelloWorld.java
```

Write:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
        System.out.println("I am learning DevOps.");
    }
}
```

Compile:

```bash
javac HelloWorld.java
```

Check:

```bash
ls
```

You should see:

```text
HelloWorld.java
HelloWorld.class
```

Run:

```bash
java HelloWorld
```

---

## Lab 6 — Node.js

Install NVM and Node.js following the current official Node.js instructions.

Then verify:

```bash
node -v
npm -v
```

Create:

```bash
nano hello.js
```

Write:

```javascript
console.log("Hello from Node.js");
```

Run:

```bash
node hello.js
```

---

# 48. Mini Challenge

Without looking at the commands above, complete this:

### Challenge

Create:

```text
devops/
└── day2/
    ├── python/
    │   └── hello.py
    ├── java/
    │   └── HelloWorld.java
    └── node/
        └── hello.js
```

Python should print:

```text
Hello from Python
```

Java should print:

```text
Hello from Java
```

Node.js should print:

```text
Hello from Node.js
```

Then use:

```bash
tree
```

to verify the structure.

---

# 49. Day 2 Command Cheat Sheet

## Package Management

```bash
sudo apt update
sudo apt upgrade -y
sudo apt list --upgradable
sudo apt search <package>
sudo apt install <package>
sudo apt remove <package>
```

## Linux Navigation

```bash
pwd
ls
ls -la
cd /
cd
cd ..
```

## Files

```bash
touch file.txt
nano file.txt
cat file.txt
```

## History

```bash
history
history -c
```

## Privileges

```bash
sudo command
sudo -i
sudo su
```

## Python

```bash
python3 --version
python3 hello.py
```

## Java

```bash
javac HelloWorld.java
java HelloWorld
```

## Node.js

```bash
node -v
npm -v
node hello.js
```

## Git

```bash
git --version
```

---

# 50. Most Important Concepts to Remember

```text
                 SOFTWARE
                    |
          +---------+---------+
          |                   |
       Windows              Linux
          |                   |
      Chocolatey             APT
          |                   |
    choco install       apt install
          |                   |
          +---------+---------+
                    |
                DEVOPS
                    |
          +---------+---------+
          |         |         |
        Python     Java     Node.js
          |         |         |
       Runtime     JVM     Node Runtime
```

---

# 51. Day 2 Interview Revision

Before moving to Day 3, I should be able to answer:

1. What is a package manager?
2. Why do we need package managers?
3. What package manager does Ubuntu use?
4. What is APT?
5. What is Chocolatey?
6. What is DNF?
7. What is YUM?
8. What is the difference between `apt update` and `apt upgrade`?
9. What does `history` do?
10. What is the root user?
11. What does `sudo` do?
12. What is the difference between `sudo` and `su`?
13. What do `$` and `#` usually indicate?
14. What is the difference between JDK, JRE and JVM?
15. What does `javac` do?
16. What does `java` do?
17. Why must a public Java class match the filename?
18. How does Python execution differ from Java compilation?
19. What is Node.js?
20. What is NVM?
21. What is npm?
22. Why is package management important in DevOps?

---

# 52. Day 2 Key Takeaways

```text
DAY 2
 │
 ├── Windows Tools
 │    └── Chocolatey
 │
 ├── Linux Package Management
 │    └── APT
 │
 ├── Linux Administration
 │    ├── sudo
 │    ├── root
 │    ├── history
 │    └── filesystem
 │
 ├── Development Tools
 │    ├── Git
 │    ├── Nano
 │    ├── Python
 │    ├── Java
 │    └── Node.js
 │
 └── Programming Execution
      ├── Python → Runtime
      ├── Java → javac → .class → JVM
      └── Node.js → JavaScript Runtime
```

---

# 53. Day 2 Success Criteria

Before moving to Day 3:

- [ ] I understand what a package manager is.
- [ ] I can explain APT.
- [ ] I can explain Chocolatey.
- [ ] I can install a package using APT.
- [ ] I understand `apt update`.
- [ ] I understand `apt upgrade`.
- [ ] I can use `history`.
- [ ] I understand normal user vs root.
- [ ] I understand `sudo`.
- [ ] I can install Python.
- [ ] I can run a Python program.
- [ ] I can install Java.
- [ ] I understand `javac` vs `java`.
- [ ] I can compile and run a Java program.
- [ ] I understand JDK/JRE/JVM at a basic level.
- [ ] I understand what Node.js is.
- [ ] I understand what NVM is.
- [ ] I can check Git/Python/Java/Node versions.
- [ ] I can explain these concepts in an interview.

---
