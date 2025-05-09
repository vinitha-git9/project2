# Linux Kernel and System Basics

## WEEK-1

---

## Kernel

The "father of the kernel," specifically referring to the Linux kernel, is **Linus Torvalds**.

The kernel is essentially the core of an operating system.  
It is the fundamental layer between computer hardware and software.  
It interacts with hardware and is often considered the nucleus of a computer’s operating system.  
It manages system resources such as the processor, memory, and device drivers.  
The kernel is stored in the computer’s memory.

### Types of Kernel

- **Monolithic Kernels**:  
  Entire OS including schedulers, file system, and device drivers is part of the kernel.  
  Can lead to complexity and potential instability if a service fails.

- **Microkernels**:  
  Run minimal services in kernel space for OS functionality.  
  Improves reliability and security.

- **Hybrid Kernels**:  
  Combine the advantages of monolithic and microkernels.  
  Some services run in kernel space, others in user space.

---

## Linux Distributions

A Linux distribution (or *distro*) is a complete operating system built around the Linux kernel.

### Components of a Linux Distribution

- Linux Kernel
- GNU Tools & Libraries
- Package Management System
- Software Applications
- Graphical User Interface (GUI)
- System Libraries & Utilities
- Bootloader

### Types of Distributions

- **General-Purpose**: Ubuntu, Fedora, Linux Mint  
- **Enterprise**: RHEL, CentOS, SUSE  
- **Security-Focused**: Kali Linux, Parrot OS  
- **Lightweight**: Lubuntu, Puppy Linux  
- **Specialized**: Ubuntu Studio, Edubuntu, OpenWrt

### ✅ Why Use a Linux Distribution?

- Open Source and Free
- Secure
- Customizable
- Ideal for Development
- Resource-Efficient
- Wide Variety
- Stable & Reliable
- Community Support

---

## Linux File System

### 🧩 Key Linux User Management Commands

#### Creating Users

```bash
sudo useradd -m -s /bin/bash username
sudo adduser username
```

#### Modifying Users

```bash
sudo usermod -aG groupname username
```

#### Deleting Users

```bash
sudo userdel username
```

#### Managing Passwords

```bash
sudo passwd username
sudo passwd -l username   # Lock
sudo passwd -u username   # Unlock
```

#### Managing Groups

```bash
sudo groupadd groupname
sudo groupdel groupname
sudo gpasswd -a username groupname
```

---

## File Permissions

### Symbolic Representation

`-rwxr-xr--`

- First character: File type (`-` for file, `d` for directory)
- Next 3: Owner (read, write, execute)
- Next 3: Group
- Next 3: Others

### Numeric Representation

- `r = 4`, `w = 2`, `x = 1`
- Example: `755` = Owner: rwx, Group: r-x, Others: r-x

### Changing Permissions

```bash
chmod u+x file
chmod 755 file
chown user:group file
umask 022
```

---

## Processes and Job Management

### Viewing Processes

```bash
ps aux
ps -ef
ps -u username
top
htop
```

### Process States

- **Running**
- **Sleeping**
- **Stopped**
- **Zombie**

### Signals

- `SIGINT (2)`: Ctrl+C
- `SIGTERM (15)`: Termination
- `SIGKILL (9)`: Force kill
- `SIGSTOP`: Pause
- `SIGCONT`: Resume

### Job Control

```bash
bg       # background
fg       # foreground
jobs     # list
kill PID
Ctrl+Z   # suspend
Ctrl+C   # terminate
```

---

## Package Management

### Software Distribution

- Packages include binaries, config, and metadata.

### Package Repositories

- Remote servers with software packages.

### Dependencies

- Required libraries that are automatically resolved during installation.

---

## File and Directory Management

```bash
ls       # List
pwd      # Print working directory
cd       # Change directory
mkdir    # Make directory
rmdir    # Remove directory
rm       # Remove file/directory
cp       # Copy
mv       # Move/Rename
```

---

## File Viewing and Editing

```bash
cat filename
less filename
nano filename
vim filename
```

---

## System Information

```bash
top
df
free
uname -a
```

---

## Networking

```bash
ping google.com
ifconfig
ip a
wget http://example.com
```

---

## Init System

- The **init system** is the first process (PID 1).
- It initializes system services, manages targets/runlevels, and handles shutdown.

---

## Log Files in `/var/log`

- System Logs
- Authentication Logs
- Cron Logs
- Mail Logs
- Application Logs
- Background Services Logs

---

## Cron Jobs

```
* * * * * <command>
| | | | |
| | | | +---- Day of week (0-6)
| | | +------ Month (1-12)
| | +-------- Day of month (1-31)
| +---------- Hour (0-23)
+------------ Minute (0-59)
```

### Examples

```bash
0 5 * * * /path/to/script.sh       # Daily at 5:00 AM
0 15 * * 1 /path/to/script.sh      # Mondays at 3:00 PM
*/5 * * * * /path/to/script.sh     # Every 5 minutes
```

### Commands

```bash
crontab -l   # View
crontab -e   # Edit
crontab -r   # Remove
```

---

## Common Linux Commands

| Command | Description                | Example                        |
|---------|----------------------------|--------------------------------|
| ls      | List directory contents    | `ls -l`                        |
| pwd     | Print working directory    | `pwd`                          |
| cd      | Change directory           | `cd /home/user`               |
| mkdir   | Create new directory       | `mkdir new_folder`           |
| rmdir   | Remove empty directory     | `rmdir old_folder`           |
| cp      | Copy files or directories  | `cp source.txt dest.txt`     |
| mv      | Move or rename files       | `mv old.txt new.txt`         |
| rm      | Remove files or directories| `rm file.txt`                |
| touch   | Create empty file          | `touch newfile.txt`          |
| ln      | Create links               | `ln -s target.txt link.txt`  |
