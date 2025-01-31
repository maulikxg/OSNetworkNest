# Linux Concepts

---
## Memory Concepts

### Virtual Memory
Virtual memory is a memory management technique that provides an abstraction of the storage resources available to a process. Key aspects include:

- Each process gets its own virtual address space, isolated from other processes
- Allows programs to use more memory than physically available RAM
- Memory is divided into fixed-size units called pages
- Pages can be stored either in RAM or on disk (swap space)
- The Memory Management Unit (MMU) handles translation between virtual and physical addresses
- Page tables maintain mappings between virtual and physical memory addresses

### Swap Memory
Swap space is a portion of the hard drive used as an extension of RAM:

- Used when physical RAM becomes full
- Slower than RAM but prevents out-of-memory situations
- Managed by the kernel's swap subsystem
- Can be a dedicated partition or a swap file
- Multiple swap spaces can be used with different priorities

#### Common Swap Commands
```bash
# View swap usage
free -h
swapon --show

# Create a swap file
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# Enable/disable swap
sudo swapon -a  # Enable all swap in /etc/fstab
sudo swapoff -a # Disable all swap
```


### Log Rotation
LogRotate handles automatic rotation and compression of logs:

- Configuration in `/etc/logrotate.conf`
- Individual configurations in `/etc/logrotate.d/`
- Rotates logs based on size or time
- Can compress old logs
- Maintains specified number of backup copies

### Monitoring Tools

#### watch
```bash
# Monitor command output every 2 seconds
watch free -m

# Custom interval (5 seconds)
watch -n 5 'df -h'

# Highlight differences
watch -d 'ps aux | grep nginx'
```


## Best Practices

### Memory Management
1. Monitor memory usage regularly
2. Set appropriate swap size (typically 1.5x RAM for systems <2GB, 1x RAM for 2-8GB)
3. Adjust swappiness via `/proc/sys/vm/swappiness`
4. Use tools like `vmstat` and `free` to track memory usage

### Log Management
1. Implement log rotation
2. Monitor disk space used by logs
3. Set appropriate retention periods
4. Use centralized logging for multiple servers
5. Regularly review critical logs
6. Configure appropriate permissions

### Performance Monitoring
1. Set up regular monitoring
2. Create baselines for normal operation
3. Configure alerts for abnormal conditions
4. Archive historical performance data
5. Document troubleshooting procedures
---

---
# **Linux System Logs: Summary & Recap**

System logs (`syslogs`) help diagnose **system events, errors, hardware issues, and security logs**. Below is a summarized guide to checking and managing system logs in Linux.

---

### Main Log Locations
- `/var/log/syslog` - General system messages
- `/var/log/auth.log` - Authentication events
- `/var/log/kern.log` - Kernel messages
- `/var/log/dmesg` - Boot messages
- `/var/log/messages` - Global system messages
- `/var/log/apache2/` - Apache web server logs
- `/var/log/mysql/` - MySQL database logs

### Log Monitoring Commands


## **📌 1. General System Logs**

### **🛠 View System Logs**
```bash
journalctl  #View all logs (systemd)

# View all logs
journalctl

# Follow new messages
journalctl -f

# Show logs since last boot
journalctl -b

# Show logs for specific service
journalctl -u nginx.service

# Show logs since specific time
journalctl --since "2025-01-30 09:00:00"

tail -f /var/log/syslog  #Follow log file in real-time


tail -n 100 /var/log/auth.log  #Show last n lines


cat /var/log/syslog  # General logs (Ubuntu/Debian)
```

### **🛠 View Logs for a Specific Service**
```bash
journalctl -u sshd  # Replace 'sshd' with any service name
```

### **🛠 View Logs in Real-Time**
```bash
journalctl -f  # Follow live logs
tail -f /var/log/syslog  # Alternative method
```

---

## **📌 2. Authentication & Security Logs**

### **🛠 Check Login Attempts & Authentication Failures**
```bash
cat /var/log/auth.log  # Ubuntu/Debia
```

### **🛠 View Failed SSH Logins**
```bash
grep "Failed password" /var/log/auth.log
```

---

## **📌 3. Kernel Logs (Hardware & Drivers)**

### **🛠 View Kernel Logs**
```bash
# View kernel ring buffer
dmesg

# Follow new messages
dmesg -w

# Show human-readable timestamps
dmesg -T
dmesg | less  # General kernel logs
journalctl -k  # Kernel logs in systemd
tail -n 50 /var/log/kern.log  # Last 50 kernel logs
```

### **🛠 Check Kernel Errors**
```bash
dmesg | grep -i "error"
```

### **🛠 View Kernel Logs from Last Boot**
```bash
journalctl -k -b
```

---

## **📌 4. Disk & File System Logs**

### **🛠 Check Disk Errors**
```bash
journalctl -k | grep -i "error"
sudo smartctl -H /dev/sdX  # Check disk health (replace X with disk name)
```

### **🛠 Check Filesystem Corruption & Repair**
```bash
sudo fsck -n /dev/sdX  # Check without fixing
sudo fsck -y /dev/sdX  # Automatically fix errors
```

---

## **📌 5. Searching & Filtering Logs**

### **🛠 Search Logs for Errors**
```bash
grep -i "error" /var/log/syslog
journalctl | grep -i "failed"
```

### **🛠 View Logs Between Specific Dates**
```bash
journalctl --since "2024-01-01" --until "2024-01-31"
```

---

## **📌 Summary: Quick Commands**
| **Task** | **Command** |
|----------|------------|
| View system logs | `journalctl` or `cat /var/log/syslog` |
| View logs from last boot | `journalctl -b` |
| View real-time logs | `journalctl -f` or `tail -f /var/log/syslog` |
| View authentication logs | `cat /var/log/auth.log` |
| Check kernel logs | `dmesg | less` or `journalctl -k` |
| Find errors in logs | `grep -i "error" /var/log/syslog` |
| View logs for a service | `journalctl -u sshd` |
| Check disk health | `sudo smartctl -H /dev/sdX` |

---


