# DevOps Day 06 — Linux Users, Groups, Ownership, Permissions & Docker

## 🎯 Learning Objectives

- Understand Linux users and groups
- Work with `/etc/passwd` and `/etc/group`
- Create users and groups
- Add users to groups with `usermod`
- Check identity with `id` and `groups`
- Manage file ownership with `chown` and `chgrp`
- Review Linux permissions and ACLs
- Understand SUID, SGID and Sticky Bit
- Install and manage Docker on Amazon Linux
- Pull and run an Nginx container
- Understand Docker port mapping
- Troubleshoot a containerized web server

---

# 1. Linux Users & Groups

Linux is a multi-user operating system.

Every file normally has:

```text
Owner (User)
Group
Others
```

Concept:

```text
                 Linux File
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
        User       Group      Others
          │          │          │
         rwx        r-x        r--
```

Groups are useful because permissions can be managed for many users together.

---

# 2. Update Amazon Linux

```bash
sudo yum update -y
sudo dnf upgrade -y
sudo yum upgrade -y
sudo yum list --upgradable
```

> Amazon Linux versions may support both `yum` and `dnf`.

---

# 3. Root User

Switch to root:

```bash
sudo su
```

Check:

```bash
whoami
```

Exit:

```bash
exit
```

Usually:

```text
$ → normal user
# → root
```

Use root privileges only when required.

---

# 4. `/etc/passwd`

View user account information:

```bash
cat /etc/passwd
```

Example:

```text
tanmay:x:1001:1001::/home/tanmay:/bin/bash
```

Important fields include:

```text
Username
UID
GID
Home directory
Login shell
```

Actual password hashes are generally stored in:

```text
/etc/shadow
```

not directly in `/etc/passwd`.

---

# 5. `/etc/group`

View groups:

```bash
cat /etc/group
```

Example:

```text
dev:x:1002:tanmay
```

It contains group information and group membership.

---

# 6. Create a Group

```bash
sudo groupadd dev
```

Verify:

```bash
getent group dev
```

or:

```bash
cat /etc/group
```

---

# 7. Create a User

Basic:

```bash
sudo useradd tanmay
```

For a normal login user with a home directory:

```bash
sudo useradd -m -s /bin/bash tanmay
```

Options:

```text
-m → create home directory
-s → specify login shell
```

Set password:

```bash
sudo passwd tanmay
```

---

# 8. Add User to Group

```bash
sudo usermod -aG dev tanmay
```

Meaning:

```text
usermod → modify user
-a      → append
-G      → supplementary group
```

Important:

> `-aG` appends the group. Without `-a`, you can unintentionally replace existing supplementary group memberships.

Verify:

```bash
id tanmay
groups tanmay
```

---

# 9. Switch User

```bash
su - tanmay
```

Verify:

```bash
whoami
id
pwd
```

Return:

```bash
exit
```

---

# 10. Why Groups Matter in DevOps

Instead of assigning permissions separately:

```text
Tanmay → permission
Rahul  → permission
Atul   → permission
```

Create a group:

```text
             dev group
            /    |    \
       Tanmay  Rahul  Atul
```

Then assign access to the group.

This makes access management easier and scalable.

---

# 11. File Ownership

Create a file:

```bash
touch a.txt
ls -la
```

Example:

```text
-rw-r--r-- 1 ec2-user ec2-user 0 Aug 29 a.txt
```

The owner and group are:

```text
Owner → ec2-user
Group → ec2-user
```

---

# 12. `chown` — Change Owner

```bash
sudo chown tanmay a.txt
```

Verify:

```bash
ls -la a.txt
```

Change owner and group together:

```bash
sudo chown tanmay:dev a.txt
```

---

# 13. `chgrp` — Change Group

```bash
sudo chgrp dev a.txt
```

Verify:

```bash
ls -la a.txt
```

Comparison:

| Command | Purpose |
|---|---|
| `chown tanmay file` | Change owner |
| `chown tanmay:dev file` | Change owner + group |
| `chgrp dev file` | Change group |

---

# 14. Move File to User Home

```bash
sudo mv a.txt /home/tanmay/
```

Switch:

```bash
su - tanmay
```

Then:

```bash
cd
ls -la
```

---

# 15. Linux Permissions Refresher

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
-   rwx   r-x   r--
│    │     │     │
│   User  Group Others
│
File
```

Values:

```text
r = 4
w = 2
x = 1
```

Common permissions:

```text
755 = rwxr-xr-x
644 = rw-r--r--
700 = rwx------
600 = rw-------
400 = r--------
```

---

# 16. Symbolic `chmod`

Add execute permission to owner:

```bash
chmod u+x script.sh
```

Remove group write:

```bash
chmod g-w file.txt
```

Add read for others:

```bash
chmod o+r file.txt
```

Where:

```text
u = user/owner
g = group
o = others
a = all
```

---

# 17. Recursive Permissions & Ownership

```bash
chmod -R 755 project/
```

```bash
sudo chown -R tanmay:dev project/
```

> Use recursive commands carefully. Applying them to the wrong directory can cause serious permission problems.

---

# 18. SUID, SGID & Sticky Bit

## SUID

An executable with SUID can run with the file owner's effective privileges.

```bash
sudo chmod u+s file
```

Check:

```bash
ls -l file
```

Possible output:

```text
-rwsr-xr-x
```

## SGID

For directories, SGID makes newly created files inherit the directory's group.

```bash
sudo chmod g+s /opt/mydir
```

Check:

```bash
ls -ld /opt/mydir
```

## Sticky Bit

Commonly used for shared directories such as `/tmp`.

```bash
sudo chmod +t /tmp/mydir
```

Check:

```bash
ls -ld /tmp/mydir
```

Possible output:

```text
drwxrwxrwt
```

---

# 19. ACL — Advanced Permissions

ACL means **Access Control List**.

It provides more fine-grained permissions for specific users/groups.

Install on Amazon Linux if required:

```bash
sudo yum install acl -y
```

Give Tanmay access:

```bash
sudo setfacl -m u:tanmay:rwx file.txt
```

View:

```bash
getfacl file.txt
```

Remove:

```bash
setfacl -x u:tanmay file.txt
```

---

# 🐳 20. Docker — DevOps Use Case

Docker is a containerization platform.

Basic architecture:

```text
AWS EC2
   │
   ↓
Amazon Linux
   │
   ↓
Docker Engine
   │
   ├── Nginx Container
   ├── App Container
   └── Database Container
```

A container packages an application and its required runtime components into an isolated environment.

---

# 21. Install Docker on Amazon Linux

```bash
sudo yum install docker -y
```

Verify:

```bash
docker --version
```

---

# 22. Start & Enable Docker

Start:

```bash
sudo systemctl start docker
```

Enable at boot:

```bash
sudo systemctl enable docker
```

Check:

```bash
sudo systemctl status docker
```

---

# 23. Docker Image vs Container

### Image

A read-only template used to create containers.

### Container

A running or stopped instance created from an image.

Simple analogy:

```text
Image     = Template
Container = Instance
```

---

# 24. Docker User Permissions

Add `ec2-user` to Docker group:

```bash
sudo usermod -aG docker ec2-user
```

Apply the group in the current shell:

```bash
newgrp docker
```

Then test:

```bash
docker images
```

If necessary, log out and reconnect so the new group membership is loaded.

> Avoid running Docker as root when normal Docker group access is appropriate.

---

# 25. Pull Nginx Image

```bash
docker pull nginx
```

Check:

```bash
docker images
```

---

# 26. Run Nginx Container

```bash
docker run -d -p 80:80 --name nginx-container nginx
```

Breakdown:

```text
docker run       → create/start container
-d               → detached/background mode
-p 80:80         → host port : container port
--name           → container name
nginx            → image
```

Architecture:

```text
Internet
   │
   ↓
EC2 Public IP : 80
   │
   ↓
Docker Host : 80
   │
   ↓
Container : 80
   │
   ↓
Nginx
```

---

# 27. Check Containers

Running containers:

```bash
docker ps
```

or:

```bash
docker container ls
```

All containers:

```bash
docker ps -a
```

---

# 28. Access Nginx from Browser

Open:

```text
http://<EC2-Public-IP>
```

Example:

```text
http://54.x.x.x
```

Your EC2 Security Group must allow inbound TCP port `80` for public testing.

---

# 29. Docker Troubleshooting

If the website does not open:

```text
Browser
   ↓
EC2 Public IP
   ↓
Security Group Port 80
   ↓
Docker Port Mapping
   ↓
Nginx Container
   ↓
Nginx
```

Check EC2:

```text
Instance = Running
```

Check Security Group:

```text
Inbound TCP 80
```

Check Docker:

```bash
sudo systemctl status docker
```

Check container:

```bash
docker ps
```

Check port mapping:

```bash
docker ps
```

Look for:

```text
0.0.0.0:80->80/tcp
```

Check logs:

```bash
docker logs nginx-container
```

---

# 30. Useful Docker Commands

```bash
docker images
docker pull nginx

docker run -d -p 80:80 --name nginx-container nginx

docker ps
docker ps -a

docker logs nginx-container
docker logs -f nginx-container

docker inspect nginx-container

docker stop nginx-container
docker start nginx-container
docker rm nginx-container
```

---

# 31. Docker Cleanup

Stop:

```bash
docker stop nginx-container
```

Remove:

```bash
docker rm nginx-container
```

Remove unused Docker resources:

```bash
docker system prune -a
```

> `docker system prune -a` can remove unused images, stopped containers and other unused resources. Use it carefully.

---

# 32. Complete Day 6 Lab

```bash
# Update
sudo yum update -y

# Group
sudo groupadd dev

# User
sudo useradd -m -s /bin/bash tanmay
sudo passwd tanmay

# Add user to group
sudo usermod -aG dev tanmay

# Verify
id tanmay
groups tanmay

# File
touch a.txt
ls -la

# Ownership
sudo chown tanmay:dev a.txt
ls -la a.txt

# Move
sudo mv a.txt /home/tanmay/

# Switch user
su - tanmay
whoami
id
pwd
ls -la

# Return
exit

# Docker
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker

# Docker group
sudo usermod -aG docker ec2-user
newgrp docker

# Nginx
docker pull nginx
docker images

docker run -d -p 80:80 --name nginx-container nginx

# Verify
docker ps
docker ps -a

# Browser
# http://<EC2-Public-IP>
```

---

# 33. Interview Questions

### Q1. What is the difference between a user and a group?

> A user represents an individual account, while a group is a collection of users. Groups make permission management easier.

### Q2. What is `/etc/passwd`?

> It contains local Linux user-account information such as username, UID, GID, home directory and login shell.

### Q3. What is `/etc/group`?

> It contains local Linux group information and group memberships.

### Q4. How do you create a group?

```bash
sudo groupadd dev
```

### Q5. How do you add an existing user to a group?

```bash
sudo usermod -aG dev tanmay
```

### Q6. What does `-aG` mean?

> `-G` specifies supplementary groups and `-a` appends the new group without replacing existing supplementary groups.

### Q7. What is `chown`?

> `chown` changes the ownership of a file or directory.

Example:

```bash
sudo chown tanmay:dev file.txt
```

### Q8. What is `chgrp`?

> `chgrp` changes the group ownership of a file or directory.

### Q9. Difference between `chown` and `chgrp`?

> `chown` can change the owner and group, while `chgrp` specifically changes the group.

### Q10. What is Docker?

> Docker is a containerization platform used to package and run applications in isolated containers.

### Q11. What is a Docker image?

> A Docker image is a read-only template used to create containers.

### Q12. What is a Docker container?

> A container is an instance created from a Docker image that can run an application.

### Q13. How do you install Docker on Amazon Linux?

```bash
sudo yum install docker -y
```

### Q14. How do you start Docker?

```bash
sudo systemctl start docker
```

### Q15. How do you enable Docker at boot?

```bash
sudo systemctl enable docker
```

### Q16. How do you pull Nginx?

```bash
docker pull nginx
```

### Q17. How do you run Nginx on port 80?

```bash
docker run -d -p 80:80 --name nginx-container nginx
```

### Q18. What does `-p 80:80` mean?

> It maps port 80 on the Docker host to port 80 inside the Nginx container.

---

# ⭐ 34. Scenario-Based Interview Question

**Interviewer:** You started an Nginx Docker container, but the website is not opening. What will you check?

### Strong Answer

> "First, I would confirm that the EC2 instance is running. Then I would check the Security Group to make sure inbound TCP port 80 is allowed. Next, I would check the Docker service with `systemctl status docker`. Then I would run `docker ps` to confirm that the Nginx container is running and verify the `80:80` port mapping. Finally, I would check `docker logs nginx-container` for container-level errors."

This demonstrates a structured troubleshooting approach.

---

# ⭐ 35. More Scenario Questions

### Scenario 1 — Docker Permission Denied

**Problem:** `docker ps` gives permission denied after adding the user to the Docker group.

Check:

```bash
id
```

Apply the group:

```bash
newgrp docker
```

Or log out and reconnect.

---

### Scenario 2 — User Cannot Access a File

Check:

```bash
ls -la file.txt
```

Then investigate:

- Owner
- Group
- Permission bits
- Parent-directory permissions
- ACLs

For ACL:

```bash
getfacl file.txt
```

---

### Scenario 3 — Nginx Container Stops

Check:

```bash
docker ps -a
```

Then:

```bash
docker logs nginx-container
```

The goal is to identify the reason instead of repeatedly restarting the container.

---

# 36. Day 6 Cheat Sheet

```bash
# Users
cat /etc/passwd
sudo useradd -m -s /bin/bash tanmay
sudo passwd tanmay
su - tanmay
whoami
id
exit

# Groups
cat /etc/group
sudo groupadd dev
sudo usermod -aG dev tanmay
id tanmay
groups tanmay

# Ownership
touch a.txt
sudo chown tanmay a.txt
sudo chown tanmay:dev a.txt
sudo chgrp dev a.txt
sudo mv a.txt /home/tanmay/

# Permissions
ls -la
chmod 755 script.sh
chmod u+x script.sh
chmod g-w file.txt
chmod o+r file.txt

# Docker
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker

sudo usermod -aG docker ec2-user
newgrp docker

docker images
docker pull nginx

docker run -d -p 80:80 --name nginx-container nginx

docker ps
docker ps -a
docker logs nginx-container

docker stop nginx-container
docker rm nginx-container

docker system prune -a
```

---

# 37. Day 6 Success Checklist

- [ ] Understand users and groups
- [ ] Understand `/etc/passwd`
- [ ] Understand `/etc/group`
- [ ] Create a user
- [ ] Create a group
- [ ] Add a user to a group
- [ ] Use `id`
- [ ] Use `groups`
- [ ] Switch users using `su`
- [ ] Understand file ownership
- [ ] Use `chown`
- [ ] Use `chgrp`
- [ ] Understand `chmod`
- [ ] Understand SUID
- [ ] Understand SGID
- [ ] Understand Sticky Bit
- [ ] Understand ACL basics
- [ ] Install Docker
- [ ] Start and enable Docker
- [ ] Add a user to the Docker group
- [ ] Pull a Docker image
- [ ] Run a Docker container
- [ ] Understand port mapping
- [ ] Deploy Nginx using Docker
- [ ] Troubleshoot a containerized web server
- [ ] Clean up Docker resources

---

# 🎤 38. Interview Answer — "What Did You Learn Today?"

> "Today I learned Linux user and group management, file ownership and permission management. I practiced creating users and groups, adding users to groups with `usermod`, and changing file ownership using `chown` and `chgrp`. I also learned advanced concepts such as SUID, SGID and ACLs. For the DevOps part, I installed Docker on an Amazon Linux EC2 instance, managed the Docker service using systemd, pulled the Nginx image and deployed it as a container using port mapping. I also learned a basic troubleshooting flow involving Security Groups, Docker service status, container status, port mappings and container logs."

---

# 🚀 39. Day 6 DevOps Connection

```text
AWS EC2
   │
   ↓
Amazon Linux
   │
   ├── Users
   │     └── Permissions
   │
   ├── Groups
   │     └── Shared Access
   │
   ├── Ownership
   │     └── chown / chgrp
   │
   └── Docker
          │
          ↓
       Nginx Image
          │
          ↓
     Nginx Container
          │
          ↓
       Port 80
          │
          ↓
      Web Browser
```

Linux administration provides the foundation, while Docker provides application containerization.

---

# 📌 40. Most Important Things to Remember

```text
/etc/passwd  → User account information
/etc/group   → Group information

useradd      → Create user
groupadd     → Create group
usermod      → Modify user
passwd       → Set password
id           → User/group identity
su           → Switch user

chown        → Change owner
chgrp        → Change group
chmod        → Change permissions

Docker image → Template
Container    → Instance of image

docker pull  → Download image
docker run   → Create/start container
docker ps    → List containers
docker logs  → View container logs

-p 80:80     → Host port 80 → Container port 80
```

---

# 📚 GitHub Structure for Your Main Learning Repository

Recommended:

```text
cloud-devops-learning/
│
├── README.md
│
├── 01-linux/
│   ├── day-01-linux-fundamentals/
│   │   └── README.md
│   ├── day-02-linux-tools/
│   │   └── README.md
│   ├── day-03-apache-bash/
│   │   └── README.md
│   ├── day-04-aws-ec2/
│   │   └── README.md
│   ├── day-05-linux-permissions/
│   │   └── README.md
│   └── day-06-users-groups-docker/
│       └── README.md
│
├── scripts/
│   └── ...
│
└── projects/
    └── ...
```

**Never commit:**

```text
*.pem
passwords
API keys
AWS access keys
database credentials
.env files containing secrets
```

Use `.gitignore` for sensitive files.

---

# 🏁 Progress

```text
Day 01 → Linux Fundamentals
       ↓
Day 02 → Linux Tools & Package Management
       ↓
Day 03 → Apache + Bash
       ↓
Day 04 → AWS EC2 + SSH + Web Hosting
       ↓
Day 05 → Permissions + File Management + Bash + Users
       ↓
Day 06 → Users + Groups + Ownership + Docker + Nginx
       ↓
Day 07 → Next DevOps Topic
```
