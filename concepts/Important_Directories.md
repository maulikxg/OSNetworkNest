# Directories in Linux Root

[Reference](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/storage_administration_guide/ch-filesystem#s3-filesystem-dev)

## Root Directories and Their Purpose

### 1. `/boot/`
The `/boot/` directory contains static files required to boot the system, such as the Linux kernel. These files are essential for the system to boot properly.

### 2. `/dev/`
The `/dev/` directory contains device nodes that represent:
- Devices attached to the system.
- Virtual devices provided by the kernel.

These device nodes are essential for system functionality. The `udevd` daemon creates and removes device nodes in `/dev/` as needed.

### 3. `/etc/`
The `/etc/` directory is reserved for configuration files that are local to the machine. It should contain no binaries; any binaries should be moved to `/bin/` or `/sbin/`.

### 4. `/lib/`
The `/lib/` directory should only contain libraries needed to execute the binaries in `/bin/` and `/sbin/`. These shared library images are used to boot the system or execute commands within the root file system.

### 5. `/media/`
The `/media/` directory contains subdirectories used as mount points for removable media, such as USB storage devices, DVDs, and CD-ROMs.

### 6. `/mnt/`
The `/mnt/` directory is reserved for temporarily mounted file systems, such as NFS file system mounts.
- The `/mnt/` directory must not be used by installation programs.

### 7. `/proc/`
The `/proc/` directory contains special files that either extract information from the kernel or send information to it. Examples include system memory, CPU information, and hardware configuration.

### 8. `/sbin/`
The `/sbin/` directory stores binaries essential for booting, restoring, recovering, or repairing the system. The binaries in `/sbin/` require root privileges to use. Additionally, `/sbin/` contains binaries used by the system before the `/usr/` directory is mounted; any system utilities used after `/usr/` is mounted are typically placed in `/usr/sbin/`.

### 9. `/usr/`
The `/usr/` directory contains files that can be shared across multiple machines. It is often on its own partition and is mounted as read-only.

### 10. `/var/`
Since the Filesystem Hierarchy Standard (FHS) requires Linux to mount `/usr/` as read-only, any programs that write log files or need `spool/` or `lock/` directories should write them to the `/var/` directory. The FHS defines `/var/` as the location for variable data, including:
- Spool directories and files
- Logging data
- Transient and temporary files
