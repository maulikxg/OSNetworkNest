
# Process Commands in OS

Operating Systems (OS) provide a set of commands to manage processes, which are instances of running programs. These commands allow users to view, control, and manipulate processes. Below is an in-depth explanation of common process-related commands in Unix-like operating systems (e.g., Linux) and their outputs.

---

# 1. `ps` (Process Status)

The `ps` command is used to display information about active processes on a Linux system. Below is a breakdown of its common use cases and options.

---

## Basic Usage

```bash
ps [options]
```

- **`ps -e`**: Shows all processes active in the terminal.
- **`ps -ef`**: Shows detailed information about all processes.

### Example Output:

```
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Jan22 ?        00:00:03 /sbin/init splash
root           2       0  0 Jan22 ?        00:00:00 [kthreadd]
root           3       2  0 Jan22 ?        00:00:00 [rcu_gp]
```

- **UID**: User ID of the process owner.
- **PID**: Process ID.
- **PPID**: Parent Process ID.
- **C**: CPU utilization.
- **STIME**: Start time of the process.
- **TTY**: Terminal associated with the process.
- **TIME**: Total CPU time used by the process.
- **CMD**: Command that started the process.

---

## Common Use Cases

### 1. **Show Processes for a Specific User**

```bash
ps -u <username>
```

- Displays all processes owned by the specified user.

---

### 2. **Show Processes with a Specific PID**

```bash
ps -p <pid>
```

- Displays information about the process with the specified Process ID.

---

### 3. **Show Processes in a Tree Format**

```bash
ps -ejH
```

- Displays processes in a hierarchical (tree) format, showing parent-child relationships.

---

### 4. **Show All Processes**

```bash
ps -a  # All processes not associated with a terminal
ps -u  # All processes of the current user
ps -x  # All processes without a controlling terminal (background jobs)
```

---

### 5. **Show Processes with Memory and CPU Usage**

```bash
ps aux
```

- Displays detailed information about all processes, including memory and CPU usage.

#### Example Output:

```
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0 169808 13112 ?        Ss   Jan22   0:03 /sbin/init splash
root           2  0.0  0.0      0     0 ?        S    Jan22   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Jan22   0:00 [rcu_gp]
```

- **%CPU**: CPU usage percentage.
- **%MEM**: Memory usage percentage.
- **VSZ**: Virtual memory size.
- **RSS**: Resident Set Size (physical memory used).
- **STAT**: Process status (e.g., `S` = sleeping, `R` = running).

---

### 6. **Sort Processes by CPU or Memory Usage**

```bash
ps aux --sort=-%cpu  # Sort by CPU usage (descending)
ps aux --sort=-%mem  # Sort by memory usage (descending)
```

---

### 7. **Show Processes with Threads**

```bash
ps -eLf
```

- Displays information about all processes, including threads.

#### Example Output:

```
UID          PID    PPID     LWP  C NLWP STIME TTY          TIME CMD
root           1       0       1  0    1 Jan22 ?        00:00:03 /sbin/init splash
root           2       0       2  0    1 Jan22 ?        00:00:00 [kthreadd]
root           3       2       3  0    1 Jan22 ?        00:00:00 [rcu_gp]
```

- **LWP**: Lightweight Process (thread ID).
- **NLWP**: Number of threads in the process.

---

## When to Use `ps aux` vs. `ps -eLf`

| **Use Case**                           | **`ps aux`** | **`ps -eLf`** |
|----------------------------------------|--------------|---------------|
| **Monitor overall process resource usage** | ✅            | ❌             |
| **Monitor memory/CPU usage of processes** | ✅            | ❌             |
| **Find a specific process by name**     | ✅            | ❌             |
| **Monitor threads of a process**        | ❌            | ✅             |
| **Debug multithreaded applications**    | ❌            | ✅             |
| **Track thread IDs (LWP)**              | ❌            | ✅             |
| **Count threads in a process**          | ❌            | ✅             |

---

## Summary

- Use **`ps aux`** for general process monitoring, including CPU and memory usage.
- Use **`ps -eLf`** for detailed thread-level information and debugging multithreaded applications.
- Combine options like `-a`, `-u`, and `-x` to filter processes based on specific criteria.

---
# 2. `top` (Table of Processes)

## Overview
`top` is a real-time process monitoring tool that provides a dynamic view of system performance and running processes. It's essential for system administrators and developers for live system analysis and troubleshooting.

## Basic Syntax
```bash
top [options]
```

## Common Command Options
| Option | Description | Use Case |
|--------|-------------|----------|
| `-d N` | Set update interval to N seconds | When you need custom refresh rates |
| `-u username` | Show only user's processes | For user-specific monitoring |
| `-p PID` | Monitor specific process(es) | When tracking particular applications |
| `-b` | Batch mode output | For logging and scripting |
| `-n N` | Number of iterations | For limited duration monitoring |
| `-H` | Show individual threads | For thread-level analysis |

## Critical Metrics Reference Table

### System Load Thresholds
| Metric | Warning Level | Critical Level | Emergency Level |
|--------|---------------|----------------|-----------------|
| Load Average (per core) | > 1.0 | > 2.0 | > 5.0 |
| CPU Usage | > 70% | > 85% | > 95% |
| Memory Usage | > 70% | > 85% | > 95% |
| Swap Usage | > 20% | > 50% | > 80% |
| CPU Wait (wa) | > 10% | > 20% | > 30% |
| Zombie Processes | ≥ 1 | ≥ 5 | ≥ 10 |

### Process State Indicators
| State | Symbol | Description | Action Needed If High |
|-------|--------|-------------|----------------------|
| Running | R | Actively using CPU | If too many, investigate CPU contention |
| Sleeping | S | Waiting for event | Normal state for most processes |
| Interruptible Sleep | D | Uninterruptible sleep | If persistent, check I/O issues |
| Zombie | Z | Terminated but not reaped | Parent process investigation needed |
| Stopped | T | Process suspended | Check if intentional |

## Header Section Breakdown

### First Line
```
top - 14:28:00 up 1:15, 2 users, load average: 0.52, 0.58, 0.59
```
- Time: Current system time
- Uptime: System running duration
- Users: Number of users logged in
- Load Average: 1, 5, and 15-minute averages

### Tasks Section
```
Tasks: 180 total, 1 running, 179 sleeping, 0 stopped, 0 zombie
```
#### Critical Values:
- Running: Should typically be < number of CPU cores
- Zombie: Should be 0
- Stopped: Unusual if not debugging

### Memory Section
```
MiB Mem : 16384.0 total, 8192.0 free, 6144.0 used, 2048.0 buff/cache
MiB Swap: 4096.0 total, 4096.0 free, 0.0 used
```
#### Critical Values:
- Free Memory: Should be > 10% of total
- Swap Used: Should be < 20% of total
- Buff/Cache: Large values are good

### CPU States
```
%Cpu(s): 5.0 us, 2.0 sy, 0.0 ni, 92.0 id, 1.0 wa
```
| State | Description | Warning Threshold |
|-------|-------------|------------------|
| us (user) | User process time | > 70% sustained |
| sy (system) | Kernel time | > 30% sustained |
| ni (nice) | Nice'd process time | Context dependent |
| id (idle) | Idle time | < 20% sustained |
| wa (wait) | I/O wait time | > 10% sustained |

## Interactive Commands

### Process Control
| Key | Action | Use Case |
|-----|--------|----------|
| k | Kill process | Terminate misbehaving process |
| r | Renice process | Adjust process priority |
| s | Change update interval | Customize refresh rate |
| f | Select fields | Customize display columns |
| 1 | Toggle CPU cores view | Monitor individual core usage |

### Display Control
| Key | Action | Use Case |
|-----|--------|----------|
| h/? | Show help | Learn available commands |
| c | Toggle command line/name | See full command paths |
| V | Forest view | View process hierarchy |
| z | Color/mono | Toggle color support |

## Process List Columns

### Essential Fields
| Column | Description | Critical Values |
|--------|-------------|----------------|
| PID | Process ID | System: 1-1000 |
| USER | Process owner | Root processes should be minimal |
| PR | Priority | -20 to 19 (lower = higher priority) |
| NI | Nice value | -20 to 19 (lower = higher priority) |
| VIRT | Virtual memory | Context dependent |
| RES | Resident memory | Should not approach system RAM |
| SHR | Shared memory | Higher values are better |
| S | Process status | Z and D states need attention |
| %CPU | CPU usage | > 80% needs investigation |
| %MEM | Memory usage | > 30% needs investigation |
| TIME+ | CPU time used | Depends on process type |
| COMMAND | Command name | Used for process identification |

----

# 3. `kill` (Terminate Processes)
The `kill` command sends a signal to a process, typically to terminate it.

### Usage:
```bash
kill [signal] PID
```

### Common Signals:
- `SIGTERM` (15): Gracefully terminate the process (default signal).
- `SIGKILL` (9): Forcefully terminate the process.
- `SIGHUP` (1): Hang up (often used to reload configurations).

### Example:
```bash
kill -9 1234
```
- This forcefully terminates the process with PID `1234`.

---

# 4. `pkill` (Kill Processes by Name)
The `pkill` command kills processes by name or other attributes.

### Usage:
```bash
pkill [options] process_name
```

### Example:
```bash
pkill firefox
```
- This terminates all processes named `firefox`.

---

# 5. `killall` (Kill All Processes by Name)
The `killall` command kills all processes with a specific name.

### Usage:
```bash
killall [options] process_name
```

### Example:
```bash
killall chrome
```
- This terminates all processes named `chrome`.

---


# 6. `pgrep` (Find Processes by Name)
The `pgrep` command searches for processes by name and returns their PIDs.

### Usage:
```bash
pgrep process_name
```

### Example:
```bash
pgrep bash
```

**Output:**
```
1234
5678
```
- Returns PIDs of all `bash` processes.

---

# 7. `pstree` (Display Process Tree)
The `pstree` command displays processes in a tree format, showing parent-child relationships.

### Usage:
```bash
pstree
```

### Output:
```
init─┬─bash───pstree
     ├─sshd───bash
     └─chrome───5*[chrome───{chrome}]
```
- Shows the hierarchy of processes.

---

# 8. `lsof` (List Open Files)
The `lsof` command lists files opened by processes.

### Usage:
```bash
lsof [options]
```

### Example:
```bash
lsof -p 1234
```
- Lists files opened by the process with PID `1234`.

---


# 9. `nice` (Set Process Priority)
The `nice` command starts a process with a specified priority (niceness).

### Usage:
```bash
nice -n niceness_value command
```
- Niceness values range from `-20` (highest priority) to `19` (lowest priority).

### Example:
```bash
nice -n 10 ./script.sh
```
- Starts `script.sh` with a niceness of `10`.

---

# 10. `renice` (Change Process Priority)
The `renice` command changes the priority of an already running process.

### Usage:
```bash
renice niceness_value -p PID
```

### Example:
```bash
renice 5 -p 1234
```
- Changes the priority of process `1234` to `5`.

---



# 11. `jobs` (List Background Jobs)
The `jobs` command lists all background jobs in the current shell.

### Usage:
```bash
jobs
```

### Output:
```
[1]  + Running                 ./script.sh
[2]  - Stopped                 ./another_script.sh
```
- `[1]`: Job ID.
- `+`: Current job.
- `-`: Previous job.

---






