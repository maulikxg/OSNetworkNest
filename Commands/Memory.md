# Linux Memory and System Monitoring Commands


## 1. `free` - Display Memory Usage

The `free` command in Linux provides insights into the system's memory usage, including physical RAM and swap memory. It is useful for performance monitoring and troubleshooting memory issues.

### Basic Usage of `free`

```sh
maulikpuri@maulikpuri-ThinkPad-T14s-Gen-1:~$ free
              total        used        free      shared  buff/cache   available
Mem:       15991304     4147060     5833312      532304     6010932    10674492
Swap:       2097148           0     2097148

```

#### Explanation:

- **Free**: Memory that is completely unallocated.
- **Shared***: Memory that is used by multiple processes(shared libraries, shared memory segments).
- **buff/cache**: (Buffer) Used as temporary storage of data before writing to disk. (cache) is the page cache.
- **available**: This is more accurate estimate of free RAM, as it accounts for buffer aswell: **free + reclaimable(cache/buffer)**



### Human-Readable Format

```sh
maulikpuri@maulikpuri-ThinkPad-T14s-Gen-1:~$ free -h
              total        used        free      shared  buff/cache   available
Mem:           15Gi       3.2Gi       6.3Gi       395Mi       5.7Gi        11Gi
Swap:         2.0Gi          0B       2.0Gi
```

#### Why Use `-h`?

- Converts memory values into an easier-to-read format (GB, MB, KB).
- Helps quickly understand system memory allocation.

### Show Total Memory Including Swap

```sh
free -t
```

#### Explanation:

- Adds an extra row displaying the total memory (RAM + Swap) combined.

### Continuously Monitor Memory Usage Every 5 Seconds

```sh
free -h -s 5
```

#### Explanation:

- Updates memory usage every 5 seconds for real-time monitoring.

#

## Real-Life Analogy: Restaurant Kitchen & Tables 🍽️

Think of your **RAM** as a restaurant:

- **Total memory** = Total tables in the restaurant.
- **Used memory** = Tables occupied by customers.
- **Free memory** = Completely empty tables (but restaurants rarely leave tables empty!).
- **Buff/cache** = Reserved tables that are cleaned and can be used quickly.
- **Available memory** = Free + Cleaned tables (tables that can be used immediately).
- **Swap** = Extra chairs outside the restaurant (if all tables are full, people wait outside, but it's uncomfortable and slow!).

💡 **Moral:** A restaurant (Linux) keeps tables (RAM) occupied as much as possible, cleaning and reusing them when needed. This is why **free memory is low but available memory is what really matters!** 🚀



---

## 2. `vmstat` - Virtual memory statistics

The `vmstat` command (short for "virtual memory statistics") reports information about processes, memory, paging, block IO, traps, and CPU activity. It’s a very useful command to monitor your system's health and performance, especially when debugging performance issues.

### Basic Syntax:
```bash
vmstat [options] [delay [count]]
```
- **delay**: The number of seconds between each report (optional).
- **count**: The number of reports to display (optional).

## Key Columns in `vmstat` Output:



### 1. **Processes (procs):**
- **r**: The number of processes waiting for CPU time (ready to run).
- **b**: The number of processes in uninterruptible sleep (blocked processes, often waiting for I/O).

### 2. **Memory:**
- **swpd**: The amount of virtual memory used (in KB). When physical memory is exhausted, the system uses swap space on disk.
- **free**: The amount of free memory (in KB).
- **buff**: Memory used as buffers by the kernel (in KB).
- **cache**: Memory used for caching data (in KB). Cached data helps improve I/O performance.

### 3. **Swap:**
- **si**: The amount of memory swapped in from disk (in KB per second). This indicates the system is swapping memory from the disk to RAM.
- **so**: The amount of memory swapped out to disk (in KB per second). This means the system is moving data from RAM to disk to free up memory.

### 4. **I/O (Input/Output):**
- **bi**: Blocks received from a block device (in blocks per second). This shows data being read from disk.
- **bo**: Blocks sent to a block device (in blocks per second). This shows data being written to disk.

### 5. **System:**
- **in**: The number of interrupts per second, including the clock.
- **cs**: The number of context switches per second. A context switch occurs when the CPU switches from one process to another, typically when it’s done running one process and picks up another.

### 6. **CPU (in percentages):**
- **us**: The percentage of CPU time spent running user processes (non-kernel code).
- **sy**: The percentage of CPU time spent running system (kernel) code.
- **id**: The percentage of CPU time spent idle, doing nothing.
- **wa**: The percentage of CPU time spent waiting for I/O operations to complete.
- **st**: The percentage of CPU time stolen from a virtual machine (relevant in VM environments).

## Example Usage

1. **Basic Command** (Single snapshot):
   To get a snapshot of your system's resource usage, simply run:
   ```bash
   vmstat
   ```

2. **Monitoring Over Time (with delay)**:
   To get a report every 1 second, run:
   ```bash
   vmstat 1
   ```
   This will keep printing the system’s stats every second.

3. **Limit the Number of Reports**:
   To get 5 reports, 1 second apart:
   ```bash
   vmstat 1 5
   ```
   This will output 5 reports, each showing stats for a 1-second interval.

## Example Output:
Let’s take a look at an example output:
```bash
$ vmstat 1 5
procs  memory      swap          io     system     cpu
r  b   swpd  free  buff  cache  si  so  bi  bo  in  cs us sy id wa st
1  0  1024  2048  256   512   0   0   10  20  100 200  10  15 70  5  0
1  0  1024  2040  256   512   0   0   12  25  105 210  12  17 68  4  0
1  0  1024  2035  256   512   0   0   15  18  110 220  14  18 65  3  0
1  0  1024  2020  256   512   0   0   13  30  115 230  16  19 62  4  0
1  0  1024  2015  256   512   0   0   14  28  120 240  18  20 60  5  0
```

## Key Insights from the Output:
- **Processes**: In the "procs" section, `r` shows 1 process is waiting for CPU time and `b` shows no blocked processes.
- **Memory**: 
   - `swpd` (swap usage) is 1024 KB, indicating that some memory is swapped out to disk.
   - `free` memory decreases slightly each time, showing some memory is being used.
   - `buff` and `cache` remain the same, indicating no change in buffers or cached memory.
- **Swap**: No swap activity (`si` and `so` are both 0), so no memory is being swapped in or out.
- **I/O**: `bi` and `bo` show data is being transferred to and from disk, but the numbers are low, so the system is not heavily relying on disk I/O.
- **CPU**: 
   - `us` (user CPU time) is around 10%, meaning most of the CPU time is spent on user processes.
   - `sy` (system CPU time) is around 15%, indicating some kernel activity.
   - `id` (idle time) is around 70%, showing the CPU is not overly busy.
   - `wa` (waiting for I/O) is low, meaning the CPU isn't waiting much for I/O operations.

# **📊 Critical Values for `vmstat` Output**

## **🚨 Critical Thresholds in `vmstat`**

| **Category**       | **Column**         | **Normal Range**       | **Critical Value (Dangerous)**  | **Why It’s Dangerous?**                           |
|-------------------|------------------|----------------------|--------------------------------|---------------------------------|
| **👨‍💻 CPU Load**  | `r` (Runnable processes) | `≤ Number of CPU Cores` | `> CPU Cores (Consistently)`   | Indicates **CPU bottleneck**, system lagging.    |
| **🛑 Blocked Processes** | `b` (Blocked) | `0` or `1` | `> 2 for long time` | Processes stuck waiting for disk or network. |
| **📦 Swap Usage** | `swpd` (Swap Used) | `0` (Best) | `> 500MB` (High Load) | RAM is full, performance slows down drastically. |
| **📥 Swap In** | `si` (Swap In) | `0` | `> 100 KB/s consistently` | System constantly moving data from disk swap to RAM (slow). |
| **📤 Swap Out** | `so` (Swap Out) | `0` | `> 100 KB/s consistently` | System is **running out of memory** and aggressively swapping. |
| **📄 Disk Read** | `bi` (Block In) | `< 100` (Idle) | `> 1000 KB/s` | High disk read means **I/O bottleneck** (slow HDD/SSD). |
| **📝 Disk Write** | `bo` (Block Out) | `< 100` (Idle) | `> 1000 KB/s` | Heavy write load can slow down system performance. |
| **🔄 Context Switching** | `cs` (Context Switches) | `< 5000/s` | `> 10,000/s consistently` | Too many context switches cause CPU inefficiency. |
| **⏳ Interrupts** | `in` (Interrupts) | `< 1000/s` | `> 5000/s` | High interrupts mean **hardware overload** (e.g., network, storage). |
| **🔥 User CPU%** | `us` (User CPU) | `< 30%` | `> 80% consistently` | CPU is overloaded by **user processes**. |
| **⚙️ System CPU%** | `sy` (System CPU) | `< 20%` | `> 50% consistently` | Kernel is overloaded, causing slowdowns. |
| **💤 Idle CPU%** | `id` (Idle CPU) | `> 50%` (Idle is good) | `< 5% consistently` | CPU is completely maxed out. |
| **⌛ I/O Wait%** | `wa` (I/O Wait) | `0-5%` | `> 10% consistently` | CPU is waiting too long for disk I/O. |

---

## **🔥 Examples of Dangerous Situations**

### **1️⃣ CPU Bottleneck (Too Many Running Processes)**
```bash
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 8  0      0  10240  844080 6145428    0    0     6   112  5000 12000 90  5  3  2  0
```
- **`r = 8`** (If you have 4 cores, this is a problem).  
- **`us = 90%`** → CPU is overloaded by processes.  
- **Fix**: Check CPU usage with `top` or `htop`. Kill high CPU processes.

---

### **2️⃣ Swap Thrashing (System Running Out of RAM)**
```bash
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 2  1  1024000  5000  844080  102400    1000    800     6   112  2000 3000 10  5  5  75  0
```
- **`swpd = 1GB`** (Too much swap usage).  
- **`si = 1000 KB/s`, `so = 800 KB/s`** → System is thrashing (constant RAM-disk swapping).  
- **Fix**: Add RAM, reduce background apps.

---

### **3️⃣ I/O Bottleneck (Slow Disk or SSD)**
```bash
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  5  0  204800  10240  1000000  0  0  5000  6000  1500  3000  10  5  10  75  0
```
- **`b = 5`** (Many blocked processes, waiting for disk).  
- **`bi = 5000 KB/s, bo = 6000 KB/s`** (Disk is overloaded).  
- **Fix**: Check disk health with `iotop`, `df -h`, or `smartctl`.

---

## **🚀 Conclusion**
- **CPU (`r`, `us`, `sy`, `id`)** → **If `r > cores` or `us + sy > 90%`, CPU is overloaded**.  
- **Memory (`swpd`, `si`, `so`)** → **If `si` or `so` > 100 KB/s, RAM is full & swapping too much**.  
- **Disk (`bi`, `bo`)** → **If `bi` or `bo` > 1000 KB/s, disk is too busy**.  
- **Blocked Processes (`b`)** → **If `b > 2` for a long time, there are disk/network issues**.  

📌 **Action Plan:**  
- **High CPU?** → `htop` → Kill/optimize CPU-heavy processes.  
- **High Swap?** → `free -h` → Add RAM or reduce usage.  
- **High I/O?** → `iotop` → Find disk-hogging processes.  

---



## 3. `dstat` - Versatile Resource Statistics

The `dstat` command is a powerful tool for monitoring various system resources in real-time. It provides an in-depth overview of various metrics such as CPU, memory, disk, network, and I/O usage. `dstat` is a more flexible alternative to commands like `vmstat` and `iostat`.

### Basic Syntax:
```bash
dstat [options] [delay] [count]
```
- **delay**: The number of seconds between each update (optional).
- **count**: The number of reports to display (optional).

### Key Metrics in `dstat` Output:

1. **CPU:**
   - **usr**: User CPU time.
   - **sys**: System CPU time.
   - **idl**: Idle CPU time.
   - **wai**: CPU time spent waiting for I/O operations.

2. **Memory:**
   - **used**: Memory used.
   - **buff**: Memory used as buffers.
   - **cach**: Memory used as cache.
   - **free**: Free memory.

3. **Disk:**
   - **read**: Data read from the disk (KB/s).
   - **writ**: Data written to the disk (KB/s).

4. **Network:**
   - **recv**: Data received (KB/s).
   - **send**: Data sent (KB/s).

5. **Swap:**
   - **si**: Memory swapped in (KB/s).
   - **so**: Memory swapped out (KB/s).

### Example Usage:

1. **Basic Command**:
   ```bash
   dstat
   ```

2. **Monitoring Over Time (with delay)**:
   To display updates every 1 second, use:
   ```bash
   dstat 1
   ```

3. **Limit the Number of Reports**:
   To get 5 reports, 1 second apart:
   ```bash
   dstat 1 5
   ```

## Example Output:
```bash
$ dstat 1 5
---- ------ ------ ------ ------ ------ ------ ------ ------ ------
usr sys idl wai used buff cach free read writ recv send
---- ------ ------ ------ ------ ------ ------ ------ ------
10   5    80   5    3.1M  1.5M  2.5M  1.2M  0.3M  1.4M  0.6M
12   6    77   5    3.2M  1.6M  2.5M  1.4M  0.4M  1.5M  0.7M
11   5    79   5    3.1M  1.5M  2.5M  1.3M  0.3M  1.4M  0.7M
13   7    76   4    3.3M  1.6M  2.5M  1.5M  0.4M  1.6M  0.8M
12   6    77   5    3.2M  1.5M  2.6M  1.4M  0.3M  1.5M  0.7M
```

## Key Insights from the Output:
- **CPU**: 
   - `usr` (user CPU time) is around 10%, indicating user-level processes are consuming CPU time.
   - `sys` (system CPU time) is 5%, showing kernel-level processes.
   - `idl` (idle CPU time) is around 77%, indicating the CPU is not heavily loaded.
- **Memory**: 
   - Memory usage is steady with slight fluctuation, showing the system has a good balance of used and cached memory.
- **Disk**: 
   - Data is being read and written at a steady pace, with some data transfer activity, but not heavy.
- **Network**: 
   - Network activity is visible with data received and sent, showing minimal network traffic.
- **Swap**: 
   - No significant swap activity (si, so are low), so the system isn't experiencing memory pressure.




## 4. `top` & `htop`


## `top`

Displays real-time system summary information and a list of processes currently being managed by the Linux kernel.

```bash
top [options]
```

### Output Example

```
top - 14:32:01 up  2:15,  2 users,  load average: 0.00, 0.01, 0.05
Tasks: 120 total,   1 running, 119 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.3 us,  0.2 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7856.4 total,   5123.2 free,   1023.4 used,   1709.8 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   6500.2 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0  169728  13152   8400 S   0.0   0.2   0:02.34 systemd
    2 root      20   0       0      0      0 S   0.0   0.0   0:00.01 kthreadd
    3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
```

### Metrics Details

#### System Summary

| Metric                | Description                                                                 | Unit  |
|-----------------------|-----------------------------------------------------------------------------|-------|
| `up`                  | System uptime                                                              | Time  |
| `load average`        | System load averages for the last 1, 5, and 15 minutes                     | Load  |
| `Tasks`               | Total number of tasks (processes) and their states (running, sleeping, etc.) | Count |
| `%Cpu(s)`             | CPU usage breakdown (user, system, idle, etc.)                             | %     |
| `MiB Mem`             | Memory usage (total, free, used, buff/cache)                               | MiB   |
| `MiB Swap`            | Swap usage (total, free, used)                                             | MiB   |

#### Process List

| Metric | Description                                                                 | Unit  |
|--------|-----------------------------------------------------------------------------|-------|
| `PID`  | Process ID                                                                  | ID    |
| `USER` | User owning the process                                                     | Name  |
| `PR`   | Priority of the process                                                     | Level |
| `NI`   | Nice value (process priority adjustment)                                    | Value |
| `VIRT` | Total virtual memory used by the process                                    | KiB   |
| `RES`  | Resident memory (physical memory used by the process)                       | KiB   |
| `SHR`  | Shared memory used by the process                                           | KiB   |
| `S`    | Process state (e.g., `R` = Running, `S` = Sleeping, `Z` = Zombie)           | State |
| `%CPU` | Percentage of CPU used by the process                                       | %     |
| `%MEM` | Percentage of memory used by the process                                    | %     |
| `TIME+`| Total CPU time used by the process                                          | Time  |
| `COMMAND` | Command name or path                                                    | Name  |

---

## Command: `htop`

An interactive process viewer with a more user-friendly interface than `top`.

```bash
htop [options]
```

### Output Example

```
  1  [|                                                                 0.0%]   Tasks: 120, 1 thr; 1 running
  2  [|                                                                 0.0%]   Load average: 0.00 0.01 0.05
  3  [|                                                                 0.0%]   Uptime: 02:15:10
  Mem[||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||  5123M/7856M]
  Swp[                                                                0K/2048M]

  PID USER      PRI  NI  VIRT   RES   SHR S CPU% MEM%   TIME+  Command
    1 root       20   0  169M  13152  8400 S  0.0  0.2  0:02.34 systemd
    2 root       20   0     0     0     0 S  0.0  0.0  0:00.01 kthreadd
    3 root        0 -20     0     0     0 I  0.0  0.0  0:00.00 rcu_gp
```

### Metrics Details

#### System Summary

| Metric                | Description                                                                 | Unit  |
|-----------------------|-----------------------------------------------------------------------------|-------|
| `Tasks`               | Total number of tasks (processes) and their states (running, sleeping, etc.) | Count |
| `Load average`        | System load averages for the last 1, 5, and 15 minutes                     | Load  |
| `Uptime`              | System uptime                                                              | Time  |
| `Mem`                 | Memory usage (total and used)                                              | MiB   |
| `Swp`                 | Swap usage (total and used)                                                | MiB   |

#### Process List

| Metric | Description                                                                 | Unit  |
|--------|-----------------------------------------------------------------------------|-------|
| `PID`  | Process ID                                                                  | ID    |
| `USER` | User owning the process                                                     | Name  |
| `PRI`  | Priority of the process                                                     | Level |
| `NI`   | Nice value (process priority adjustment)                                    | Value |
| `VIRT` | Total virtual memory used by the process                                    | KiB   |
| `RES`  | Resident memory (physical memory used by the process)                       | KiB   |
| `SHR`  | Shared memory used by the process                                           | KiB   |
| `S`    | Process state (e.g., `R` = Running, `S` = Sleeping, `Z` = Zombie)           | State |
| `CPU%` | Percentage of CPU used by the process                                       | %     |
| `MEM%` | Percentage of memory used by the process                                    | %     |
| `TIME+`| Total CPU time used by the process                                          | Time  |
| `Command` | Command name or path                                                    | Name  |

---

### Key Differences Between `top` and `htop`

| Feature               | `top`                                      | `htop`                                     |
|-----------------------|--------------------------------------------|--------------------------------------------|
| **Interface**         | Text-based, less interactive               | Colorful, interactive, and user-friendly   |
| **Navigation**        | Limited keyboard shortcuts                 | Mouse support and more keyboard shortcuts  |
| **Customization**     | Limited                                    | Highly customizable (e.g., sort columns)   |
| **Process Tree**      | Not available                              | Displays processes in a tree view          |
| **Kill Process**      | Possible but less intuitive                | Easier with mouse or keyboard shortcuts    |

---




