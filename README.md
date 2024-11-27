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