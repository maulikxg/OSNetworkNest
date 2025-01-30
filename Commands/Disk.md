# Linux Disk Commands: Use Cases and Output Metrics

## 1. `df -h` (Disk Free Space)
**Use Case:**
- This command is used when you need to check the disk space usage on all mounted filesystems, especially to monitor whether a disk is nearing its capacity.

**Output Metrics:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        100G   30G   70G  30% /
/dev/sdb1        200G   50G  150G  25% /data
```
- `Filesystem`: The name of the filesystem or partition.
- `Size`: The total size of the filesystem.
- `Used`: How much space is used.
- `Avail`: The amount of available space.
- `Use%`: The percentage of space used.
- `Mounted on`: The directory where the filesystem is mounted.

---

## 2. `du -sh /path/to/directory` (Disk Usage for Directory)
**Use Case:**
- When you want to see the disk usage of a specific directory and its contents, this command is useful. For example, when a directory has grown unexpectedly and you want to find out why.

**Output Metrics:**
```
maulikpuri@maulikpuri-ThinkPad-T14s-Gen-1:~$ du -sh /home/maulikpuri
8.6G	/home/maulikpuri
```
- Shows the total disk usage (in this case, 8.6 GB) of the directory `/home/maulikpuri`.

---

## 3. `lsblk` (List Block Devices)
**Use Case:**
- This command is useful for getting an overview of all available storage devices (disks and partitions) on your system. It's helpful when you're managing multiple disks and need to identify them.

**Output Metrics:**
```
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  500G  0 disk
├─sda1   8:1    0   50G  0 part /
└─sda2   8:2    0  450G  0 part /data
```
- `NAME`: The device name (e.g., `sda`, `sda1`, `sda2`).
- `MAJ:MIN`: The major and minor device numbers (used internally by the kernel).
- `RM`: Indicates whether the device is removable (1 if yes, 0 if no).
- `SIZE`: The size of the device or partition.
- `RO`: Whether the device is read-only (1 if yes, 0 if no).
- `TYPE`: The type of device (`disk` for a full disk, `part` for a partition).
- `MOUNTPOINT`: The directory where the partition is mounted, or blank if not mounted.

---

## 4. `sudo fdisk -l` (List Partitions and Disks)
**Use Case:**
- When you're adding, removing, or modifying partitions, `fdisk -l` provides a detailed list of partitions and their characteristics (useful during disk partitioning).

**Output Metrics:**
```
Disk /dev/sda: 500GB
Device     Boot   Start     End   Sectors  Size Type
/dev/sda1  *     2048    1048575  1046528   500M EFI System
/dev/sda2       1048576 976773119 975724544 465G Linux filesystem
```
- `Device`: The device name.
- `Boot`: Whether the partition is bootable (`*` for bootable).
- `Start` and `End`: The start and end sectors of the partition.
- `Sectors`: The number of sectors in the partition.
- `Size`: The size of the partition.
- `Type`: The partition type (e.g., Linux filesystem, EFI System).


---


## 5. `sudo fsck /dev/sdX1` (Check Filesystem)
**Use Case:**
- This command checks and repairs filesystems, often needed if the system experiences an improper shutdown or the filesystem is corrupted.

**Output Metrics:**
```
fsck from util-linux 2.32.1
/dev/sda1: clean, 256/5242880 files, 104572/10485760 blocks
```
- `fsck from util-linux`: The version of `fsck` being used.
- `/dev/sda1`: The partition being checked.
- `clean`: Whether the filesystem is clean or has errors.
- `256/5242880 files`: The number of files in use versus the total files available.
- `104572/10485760 blocks`: The number of blocks in use versus the total blocks available.

---

## 6. `iostat -dx 1` (Disk I/O Statistics)
**Use Case:**
- When you're diagnosing disk performance, this command shows the real-time disk activity, such as read/write rates and I/O statistics. It helps to identify disks that might be underperforming.

**Output Metrics:**
```
Device            r/s   w/s  rkb/s  wkb/s  avgrq-sz avgqu-sz await svctm %util
sda               0.05  0.01   6.00   2.00     4.60     0.04    2.34  0.07  0.00
sdb               0.02  0.03   2.00   3.00     4.00     0.01    1.56  0.05  0.00
```
- `r/s` and `w/s`: Read and write requests per second.
- `rkb/s` and `wkb/s`: Read and write kilobytes per second.
- `avgrq-sz`: Average size of the requests in sectors.
- `avgqu-sz`: Average queue size of the requests.
- `await`: Average time (in milliseconds) for the requests to be processed.
- `svctm`: Average service time for the requests.
- `%util`: Percentage of time the disk was busy.

---


# Disk Usage (`du`) Commands in Linux

## 1. Basic `du` Command
```bash
du
```
**Use Case:**
- Shows disk usage of files and directories in the current directory.
- Useful when you need a quick overview of space consumption.

**Output Metrics:**
- Displays the size of each directory and subdirectory in **kilobytes (KB)** by default.

---

## 2. Display Human-Readable Sizes
```bash
du -h
```
**Use Case:**
- Provides file sizes in a readable format (**KB, MB, GB**) instead of raw kilobytes.
- Useful when checking large files.

**Example Output:**
```
4.0K    ./config
12M     ./logs
2.1G    ./videos
```

---

## 3. Show Only the Total Disk Usage of a Directory
```bash
du -sh /path/to/directory
```
**Use Case:**
- Shows only the total size of the specified directory without listing subdirectories.
- Helps when checking the overall space occupied by a specific folder.

**Example Output:**
```
5.3G    /home/user/Documents
```

---

## 4. Show Disk Usage of All Subdirectories
```bash
du -h --max-depth=1
```
**Use Case:**
- Displays the sizes of all **immediate subdirectories** in a human-readable format.
- Helps when identifying which subdirectories take up the most space.

**Example Output:**
```
512M    ./Downloads
1.5G    ./Pictures
2.0G    ./Videos
```

---


## Summary Table of `du` Commands

| Command | Description |
|---------|-------------|
| `du -h` | Show human-readable sizes |
| `du -sh /dir` | Show total size of a directory |
| `du -h --max-depth=1` | Show usage of subdirectories |
| `du -ah | sort -rh | head -10` | Find largest files |
| `du -h --exclude="*.log"` | Exclude certain file types |
| `du -ch $(find . -name "*.mp4")` | Check space used by a file type |
| `du -sh /dir1 /dir2` | Compare multiple directories |
| `du -hL` | Ignore symbolic links |
| `du -ah > file.txt` | Save output to a file |

---


