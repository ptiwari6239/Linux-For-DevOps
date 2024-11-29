# Boot Process of Linux 
### Phase 1 : BIOS/UEFI Initialization 

**The process begins when we press the power botton, the BIOS(Basic Input/Output System) or UEFI (Unified Extension firmware Interface) initializes the computer's hardware components (keyboard,screen etc) UEFI is a newer,fasteralternative to BIOS, offering improved security features like Secure Boot and support for disks larger than 2TB (unlike BIOS's MBR limitation)**

### Phase 2: POST and Boot Loader

**Next, the BIOS/UEFI performs a Power-On-Self-Test(POST) to verfiy hardware functionality. If POST is successful, the BIOS/UEFI locates and loads the boot loader software, typically checking the hard drive first then USB or CDs. the Bootloader (GRUB2) locates the operating system kernal and loads it into memory (RAM).**

### Phase 3: Kernal initialization and Systemd

**The kernal then takes over,decompressing itselft,checking hardware, and loading devices drivers and kernal modules. the init process(Systemd in modern linux system) starts, managing all other processes. Systemd handles various tasks: loadng remainng drives,mouting file systems, starting background services(networking,sound,power management),managing user logins and loading the desktop environment. Systemd uses target configuration files to determine the boot mode.**

# Linux FileSystem
### Types of files
 - Regular files : it contains programs, executable files and text files
 - Directory : it is shown in blue color, it contains list of files.
 - Special File : 
    - Blocked file (b)
    - Character devices file (c)
    - Named pipe file (p)
    - Symbolic link file (l)
    - socket file (s)
### Linux file system Hierarchy 
1. Root Directory (/) : The top-level directory in the Linux file system. All other directories are subdirectories of this root.
2. Important Directory: 
 - /bin: Contains essential executable programs and core operating system commands. This directory is often linked to /usr/bin for convenience
 - /boot: Holds files needed by the bootloader, including the kernel and initial RAM file system 
 - /dev: Contains device files that represent hardware devices connected to the system
 - /etc: Stores critical configuration files and startup scripts. For example, SSH settings are found in /etc/ssh/sshd_config
 - /home: The home directory for individual users, similar to "My Documents" in Windows. Each user has a separate directory here 
 - /lib: Contains shared libraries needed by programs 
 - /media: The mount point for removable media, such as USB drives
 - /mnt: Used for temporarily mounting devices
 - /proc: A pseudo file system that provides information about running processes and system information
 - /usr: Contains most of the user commands and utilities 
 - /var: Holds variable files such as logs and temporary files

## Inodes , Soft Links, Hard Links

### Inodes
- An inode is a data structure in a filesystem that stores metadata about a file or directory (e.g., file size, permissions, timestamps, and pointers to the actual data blocks).
- Each file or directory has a unique inode number in its filesystem.</br>
`` 
ls -i filename``

### Hard Links
-  A hard link is a direct reference (alias) to the same inode as another file. Multiple filenames can point to the same data on disk.
- Shares the same inode number as the original file.
- Changes to the content of the file reflect across all hard links.
- Deleting a hard link does not delete the data until all links are removed.
- Cannot link across different filesystems or partitions.</br>
``ln original.txt hardlink.txt``
### Soft link
- A soft link is a special type of file that points to the path of another file or directory (like a shortcut).
- Has a different inode than the original file.
- Deleting the original file leaves the soft link broken ("dangling link").
- Can link across different filesystems or partitions.</br>
``ln -s originalfile.txt softlink.txt``

# Permissions
## chmod, chown, chgrp, umask,SUID,SGID,sticky bit
### chmod
- modifies the permissions of files or directory
### chown 
-   changes the owernship  of the file or directory to a specificied user or user:group.
### chgrp 
- changes the group owernship of a file or directory
### umask 
- umask is like a default setting for file and directory permission in linux. when we create a new file or directory it decides 
what permissions should not be given to others .   
### SUID
- Allows a file to be executed with the permissions of the file's owner, regardless of who runs it.
### SGID
- Ensure that the file is executed with the permissions of the file's group.
### sticky bit
- Restricts file deletion in a directory to only the file owner,directory owner or root.even if others have write permission.
# Systemd
- Systemd is a modern init system and service manager for Linux, designed to improve boot performance and manage services, processes, and system states. Its architecture is modular and composed of multiple components working together.
## Key Components of the Systemd Architecture
1. PID 1 (Primary Init Process)

    Role: Systemd is the first process (PID 1) started by the kernel during system boot.
    Responsibilities:
    - Initializes the system.
    - Spawns and monitors services.
    - Handles system shutdown and reboot.

2. Unit Files

    Role: Define how system resources are managed. These files represent services, devices, mount points, and more.
    Types:
       - Service Units ``(.service)``: Define services ``(e.g., nginx.service)``.
       - Socket Units ``(.socket)``: Handle network or IPC sockets.
       - Timer Units ``(.timer)``: Schedule tasks (cron-like functionality).
       - Target Units ``(.target)``: Represent states ``(e.g., multi-user.target for multi-user mode)``.

3. The Journal (Logging System)

    Role: Centralized logging for all system events.
    Features:
       - Binary format for efficient storage.
       - Queryable using journalctl.

4. Dependency Management

    Role: Services and units have defined dependencies, ensuring the proper order of execution.
    Key Directives:
      -  ``After=:`` Start after a specific unit.
      -  ``Requires=:`` Depend on another unit.

## Core Systemd Utilities
1. systemctl

    - Central command for controlling services and units.

2. journalctl

    - Query logs from the journal.

3. systemd-analyze

    - Analyze system performance and boot time.
## Boot Process with Systemd
1. Kernel Initialization:

    - The Linux kernel loads and starts systemd as PID 1.

2. Target Activation:

    - Systemd loads the default target ``(e.g., multi-user.target)``.

3. Service Initialization:

    - Services are started based on their dependencies.

4. System Ready:

    - When all critical units are active, the system reaches the desired target state.

## Systemd Architecture Diagram

    Kernel
    ⬇
    Systemd (PID 1)
    ⬇
    Unit Manager
        Unit Files: .service, .socket, .timer, .target.
        Dependencies: Manages relationships between units.
        ⬇
    The Journal
        Logs for services, kernel messages, and more.
        ⬇
    User Interaction
        Tools: systemctl, journalctl, systemd-analyze.

## Basic commands for SystemD
1. Starting a service
``sudo systemctl start <serivce name>``
2. Stoping a service
``sudo systemctl stop <service name>``
3. Checking a services
``sudo systemctl status <service name>``
4. Restarting a service
``sudo systemctl restart <service name>``
5. Enabling and disabling a serivce
``sudo systemctl enable/disable <service name>``
6. Reloading configuration
 if you change a serive's configuration file,use ``sudo systemctl daemon-reload`` to apply the
  changes
7. Editing and creating service files
``sudo systemctl edit <services_name>``
## Understanding Units in Systemd
### Definition of Units 
- units are the objects that systemd manages,including services,timers,mounts and more.
A service is a specific type of unit
### Service Files
- Service files are text files that define how systemd manages a service.they contains sectiosn 
[Unit],[service], and [install],each serving different purpose.
### Structure of a service file
1. Unit section
- contains general information about the unit,such as its description and 
dependencies.
2. service section
- defines how the service operates, including the command to start it 
(ExecStart) and its type(eg, simple ,notify)
3. Install section
- configures how the unit is enabled or disabled, specifying dependencies
and run levels
### Common Directories for Unit files
- /etc/systemd/system/
     - highest priority directory for custom unit files
- /run/systemd/system/
    - contains runtime units
- /lib/systemd/systemd
    - default location for installed service files.