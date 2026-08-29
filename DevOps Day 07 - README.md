# DevOps Day 07 — Linux Shells, Text Editors & Java/Python

## Learning Objectives
- Understand Linux shells, Bash and `sh`
- Use Nano, Vi and Vim
- Understand Vim modes and save/exit commands
- Compile and run Java programs
- Run Python programs
- Understand compiler vs interpreter basics
- Use command history for troubleshooting
- Understand why text editors matter in DevOps

## 1. What Is a Shell?
A shell is a command-line interface between the user and the operating system.

```text
User → Shell → Linux Kernel → Hardware/Resources
```

Common commands:
```bash
ls
pwd
cd
mkdir
```

## 2. Bash
Bash = **Bourne Again Shell**. It is widely used for Linux administration, shell scripting and DevOps automation.

```bash
bash --version
```

## 3. `sh` and Available Shells
Check available shells:
```bash
cat /etc/shells
```

Bash provides more features than the traditional `sh` interface. Scripts intended for portability should generally use POSIX-compatible syntax.

## 4. Text Editors
Common Linux editors:
- Nano — beginner friendly
- Vi — traditional Unix editor
- Vim — improved Vi

Check versions:
```bash
vi --version
vim --version
nano --version
```

## 5. Nano
Create/open:
```bash
touch a.txt
nano a.txt
```

Type:
```text
Hello World
```

Standard Nano shortcuts:
```text
Ctrl + O → Save
Enter    → Confirm filename
Ctrl + X → Exit
```

Verify:
```bash
cat a.txt
```

> Important correction: standard Nano uses `Ctrl+O` for Write Out/save, not `Ctrl+S`.

## 6. Vi/Vim Modes
Open:
```bash
vi a.txt
# or
vim a.txt
```

Basic workflow:
```text
Normal Mode
    ↓ i
Insert Mode → type/edit text
    ↓ Esc
Normal Mode
    ↓ :wq
Save + Exit
```

Commands:
```text
i       → Insert Mode
Esc     → Normal Mode
:w      → Save
:q      → Quit
:wq     → Save and Quit
:q!     → Quit without saving
```

Example:
```bash
vi a.txt
```
Press `i`, type text, press `Esc`, type `:wq`, press Enter.

Exit without saving:
```text
Esc → :q! → Enter
```

## 7. Why Text Editors Matter in DevOps
DevOps engineers frequently edit files such as:
```text
/etc/ssh/sshd_config
/etc/httpd/conf/httpd.conf
nginx.conf
Dockerfile
docker-compose.yml
Jenkinsfile
YAML files
Shell scripts
Terraform files
Kubernetes manifests
```

Many servers do not have a graphical interface, so command-line editors are essential.

# Java

## 8. Java Execution Flow
```text
HelloWorld.java
      ↓ javac
HelloWorld.class
      ↓ JVM / java
Program Output
```

`javac` = Java compiler.
`java` = launches the compiled class using the JVM.

## 9. Install Java Development Tools on Amazon Linux
Search:
```bash
sudo yum search javac
```

A common Amazon Corretto 17 development package is:
```bash
sudo dnf install java-17-amazon-corretto-devel -y
```

Verify:
```bash
java --version
javac --version
```

Package availability can vary by Amazon Linux version/repositories.

## 10. Create Java Program
Recommended naming convention:
```bash
touch HelloWorld.java
nano HelloWorld.java
```

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Save with Nano using `Ctrl+O`, Enter, `Ctrl+X`.

## 11. Compile Java
```bash
javac HelloWorld.java
ls -la
```

A compiled file appears:
```text
HelloWorld.class
```

## 12. Run Java
```bash
java HelloWorld
```

Output:
```text
Hello, World!
```

### Important correction from the class notes
If the class is:
```java
public class helloworld
```
then run:
```bash
java helloworld
```
not `java hello`.

For a public class, the source filename must match the class name and case:
```text
HelloWorld.java
public class HelloWorld
java HelloWorld
```

# Python

## 13. Install/Check Python
```bash
sudo yum install python -y
python --version
```

The exact package name may vary by Amazon Linux version.

## 14. Create Python Program
```bash
touch helloworld.py
nano helloworld.py
```

```python
print("Hello World")
```

Run:
```bash
python helloworld.py
```

Output:
```text
Hello World
```

## 15. Java vs Python
Java is normally compiled explicitly with `javac` into JVM bytecode and then run by the JVM.

Python is commonly invoked through its interpreter; CPython also compiles source internally to bytecode as part of execution.

```text
Java:   .java → javac → .class → JVM → output
Python: .py   → Python interpreter → output
```

# Command History & Troubleshooting

## 16. View History
```bash
history
```

Useful for:
- Reviewing commands
- Learning
- Troubleshooting
- Repeating commands
- Documenting work

Clear the current shell history list:
```bash
history -c
```

> `history -c` is not a guaranteed secure deletion method because shell history may also be stored in a history file depending on configuration.

## 17. Troubleshooting `No such file or directory`
If:
```bash
cat helloworld.
```
returns:
```text
No such file or directory
```
check:
```bash
pwd
ls -la
```

Verify filename, extension, spelling, directory and case. Linux filenames are case-sensitive.

# Windows Editors

PowerShell:
```powershell
New-Item a.txt
ls
notepad a.txt
code a.txt
code .
```

With Chocolatey (if installed):
```powershell
choco install nano -y
choco install vscode -y
choco install vim -y
```

# Practical Lab — Editors

```bash
mkdir day07
cd day07
touch a.txt
nano a.txt
```
Enter `Hello from Linux`, save and exit, then:
```bash
cat a.txt
```

Edit using Vim:
```bash
vim a.txt
```
Press `i`, add `Learning DevOps`, press `Esc`, type `:wq`, Enter.

Verify:
```bash
cat a.txt
```

# Practical Lab — Java

```bash
mkdir java-project
cd java-project
sudo dnf install java-17-amazon-corretto-devel -y
java --version
javac --version
touch HelloWorld.java
nano HelloWorld.java
javac HelloWorld.java
ls -la
java HelloWorld
```

# Practical Lab — Python

```bash
mkdir python-project
cd python-project
sudo yum install python -y
python --version
touch helloworld.py
nano helloworld.py
python helloworld.py
```

# Interview Questions

### Q1. What is a shell?
A shell is a command-line interface that interprets user commands and interacts with the operating system.

### Q2. What is Bash?
Bash stands for Bourne Again Shell. It is widely used for Linux administration and automation.

### Q3. How do you check available shells?
```bash
cat /etc/shells
```

### Q4. Name three Linux text editors.
Nano, Vi and Vim.

### Q5. How do you save and exit Vim?
Press `Esc`, type `:wq`, and press Enter.

### Q6. How do you exit Vim without saving?
```text
Esc → :q! → Enter
```

### Q7. How do you save a Nano file?
`Ctrl+O`, Enter, then `Ctrl+X` to exit.

### Q8. What is `javac`?
`javac` is the Java compiler. It converts Java source code into JVM bytecode in `.class` files.

### Q9. Difference between `javac` and `java`?
`javac` compiles Java source code; `java` launches the compiled class using the JVM.

### Q10. How do you compile Java?
```bash
javac HelloWorld.java
```

### Q11. How do you run Java?
```bash
java HelloWorld
```

### Q12. How do you run Python?
```bash
python helloworld.py
```

# Scenario-Based Interview Questions

### Scenario 1 — File not found
**Interviewer:** `cat file.txt` says “No such file or directory”. What do you do?

**Answer:**
> First I check my current directory using `pwd`, then use `ls -la` to verify the filename. I check spelling, extension and case because Linux filenames are case-sensitive.

### Scenario 2 — Cannot type in Vim
**Answer:**
> Vim starts in Normal Mode. I press `i` to enter Insert Mode. After editing, I press `Esc` to return to Normal Mode.

### Scenario 3 — Java command fails after compilation
**Answer:**
> I check the generated `.class` file and confirm the class name. Java is case-sensitive, so if the class is `HelloWorld`, I run `java HelloWorld`.

### Scenario 4 — Why are editors important in DevOps?
**Answer:**
> DevOps engineers frequently manage Linux servers and edit configuration files, scripts, Dockerfiles, CI/CD files and infrastructure configuration from the command line.

# Day 7 Cheat Sheet

```bash
# Shell
cat /etc/shells
bash --version

# Editors
nano a.txt
vi a.txt
vim a.txt

# Nano
# Ctrl+O = save
# Ctrl+X = exit

# Vim
# i   = Insert Mode
# Esc = Normal Mode
# :w  = Save
# :q  = Quit
# :wq = Save + Quit
# :q! = Quit without saving

# Java
java --version
javac --version
javac HelloWorld.java
java HelloWorld

# Python
python --version
python helloworld.py

# History
history
history -c

# Troubleshooting
pwd
ls -la
cat filename
```

# Day 7 Success Checklist

- [ ] Understand shell
- [ ] Understand Bash and `sh`
- [ ] Check `/etc/shells`
- [ ] Use Nano
- [ ] Use Vi/Vim
- [ ] Understand Vim Insert and Normal modes
- [ ] Save/exit Vim
- [ ] Compile Java with `javac`
- [ ] Run Java with `java`
- [ ] Understand `.java` and `.class`
- [ ] Run Python programs
- [ ] Use command history
- [ ] Troubleshoot filename errors
- [ ] Explain text editors in a DevOps interview

# 🎤 Interview Answer — What Did You Learn Today?

> “Today I learned about Linux shells and command-line text editors. I practiced Bash, checked available shells using `/etc/shells`, and worked with Nano, Vi and Vim. I learned Vim modes and commands such as `i`, `Esc`, `:w`, `:q`, `:wq` and `:q!`. I also practiced compiling and running Java programs using `javac` and `java`, and running Python programs using the Python interpreter. From a DevOps perspective, these skills are important because engineers frequently manage Linux servers and edit configuration files, scripts, Dockerfiles and CI/CD configuration from the command line.”

# 🚀 Day 7 DevOps Connection

```text
Linux Server
     │
     ↓
Shell / Bash
     │
     ├──────────────┐
     ↓              ↓
Text Editors      Commands
     │              │
     ↓              ↓
Config Files      Automation
Scripts           Troubleshooting
Dockerfiles
YAML
Jenkinsfile
     │
     ↓
DevOps Infrastructure
```

# GitHub Structure

Add Day 7 to your main learning repository:

```text
cloud-devops-learning/
├── README.md
├── 01-linux/
│   ├── day-01-linux-fundamentals/README.md
│   ├── day-02-linux-tools/README.md
│   ├── day-03-apache-bash/README.md
│   ├── day-04-aws-ec2/README.md
│   ├── day-05-linux-permissions/README.md
│   ├── day-06-users-groups-docker/README.md
│   └── day-07-shells-text-editors/README.md
├── scripts/
└── projects/
```

For practice code, you can later add:
```text
day-07-shells-text-editors/
├── README.md
├── java/HelloWorld.java
└── python/helloworld.py
```

Never commit passwords, private keys, API keys or other secrets.

# 🏁 Progress

```text
Day 01 → Linux Fundamentals
Day 02 → Linux Tools & Package Management
Day 03 → Apache + Bash
Day 04 → AWS EC2 + SSH + Web Hosting
Day 05 → Permissions + File Management + Users
Day 06 → Users + Groups + Ownership + Docker
Day 07 → Shells + Text Editors + Java + Python
Day 08 → Next DevOps Topic
```
