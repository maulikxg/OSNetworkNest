

---

# **System Details Commands**

## **8. CPU Details**

To get detailed information about the CPU on your system, use the following commands:

### **8.1 `lscpu`** (CPU Architecture Information)
- Displays detailed information about the CPU architecture, number of cores, threads, and other related details.

#### **Command:**
```bash
lscpu
```

#### **Example Output:**
```bash
Architecture:          x86_64
CPU(s):                4
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
Model name:            Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
CPU MHz:               800.000
...
```

### **8.2 `cat /proc/cpuinfo`** (Detailed CPU Information)
- Provides detailed information about the CPU(s) such as model name, cores, cache size, etc.

#### **Command:**
```bash
cat /proc/cpuinfo
```

#### **Example Output:**
```bash
processor   : 0
vendor_id   : GenuineIntel
cpu family  : 6
model       : 142
model name  : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
...
```

### **8.3 `top` / `htop`** (Real-time CPU Usage)
- `top` provides a real-time view of CPU usage.
- `htop` is a more advanced, interactive version of `top`.

#### **Command:**
```bash
top
```

or, for `htop` (if installed):
```bash
htop
```

#### **Example Output:**
```bash
%Cpu(s):  3.4 us,  1.2 sy,  0.0 ni, 94.9 id,  0.4 wa,  0.0 hi,  0.0 si,  0.0 st
```

---

## **9. System Details (IP, Version)**

### **9.1 IP Address Details**

To get information about your system's IP address, use these commands:

#### **9.1.1 `hostname -I`** (IP Address)
- Displays the IP address assigned to your system.

##### **Command:**
```bash
hostname -I
```

##### **Example Output:**
```bash
192.168.1.10
```

#### **9.1.2 `ifconfig`** (Network Interface Configuration)
- Displays detailed network interface information, including IP addresses.

##### **Command:**
```bash
ifconfig
```

##### **Example Output:**
```bash
eth0      Link encap:Ethernet  HWaddr 00:1a:2b:3c:4d:5e
          inet addr:192.168.1.10  Bcast:192.168.1.255  Mask:255.255.255.0
          ...
```

#### **9.1.3 `ip addr show`** (Detailed IP Configuration)
- Provides detailed information about all network interfaces, including IP addresses.

##### **Command:**
```bash
ip addr show
```

##### **Example Output:**
```bash
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    inet 192.168.1.10/24 brd 192.168.1.255 scope global eth0
       valid_lft forever preferred_lft forever
```

### **9.2 System Version Information**

To get detailed information about your system version and OS:

#### **9.2.1 `lsb_release -a`** (Linux Distribution Info)
- Displays the Linux distribution name, release version, and codename.

##### **Command:**
```bash
lsb_release -a
```

##### **Example Output:**
```bash
Distributor ID: Ubuntu
Description:    Ubuntu 20.04 LTS
Release:        20.04
Codename:       focal
```

#### **9.2.2 `cat /etc/os-release`** (OS Information)
- Displays information about the operating system and its version.

##### **Command:**
```bash
cat /etc/os-release
```

##### **Example Output:**
```bash
NAME="Ubuntu"
VERSION="20.04 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
VERSION_ID="20.04"
```

#### **9.2.3 `uname -a`** (Kernel and System Information)
- Provides detailed information about the system kernel, architecture, and OS.

##### **Command:**
```bash
uname -a
```

##### **Example Output:**
```bash
Linux hostname 5.4.0-54-generic #60-Ubuntu SMP Tue Sep 29 14:51:22 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

---
