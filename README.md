# Boot Process of Linux 
### Phase 1 : BIOS/UEFI Initialization 

**The process begins when we press the power botton, the BIOS(Basic Input/Output System) or UEFI (Unified Extension firmware Interface) initializes the computer's hardware components (keyboard,screen etc) UEFI is a newer,fasteralternative to BIOS, offering improved security features like Secure Boot and support for disks larger than 2TB (unlike BIOS's MBR limitation)**

### Phase 2: POST and Boot Loader

**Next, the BIOS/UEFI performs a Power-On-Self-Test(POST) to verfiy hardware functionality. If POST is successful, the BIOS/UEFI locates and loads the boot loader software, typically checking the hard drive first then USB or CDs. the Bootloader (GRUB2) locates the operating system kernal and loads it into memory (RAM).**

### Phase 3: Kernal initialization and Systemd

**The kernal then takes over,decompressing itselft,checking hardware, and loading devices drivers and kernal modules. the init process(Systemd in modern linux system) starts, managing all other processes. Systemd handles various tasks: loadng remainng drives,mouting file systems, starting background services(networking,sound,power management),managing user logins and loading the desktop environment. Systemd uses target configuration files to determine the boot mode.**
