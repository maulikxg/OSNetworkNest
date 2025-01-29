
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

# 2. `top` (Table of Processes)
The `top` command provides a real-time, dynamic view of system processes.

### Usage:
```bash
top
```

### Output:
- Displays a summary of system resource usage (CPU, memory, etc.) at the top.
- Lists processes in order of CPU usage by default.

### Key Information:
- `PID`: Process ID.
- `USER`: Owner of the process.
- `PR`: Priority of the process.
- `NI`: Nice value (affects process priority).
- `VIRT`: Virtual memory used by the process.
- `RES`: Resident memory (physical memory used).
- `SHR`: Shared memory.
- `S`: Process status (e.g., `R` for running, `S` for sleeping).
- `%CPU`: Percentage of CPU used.
- `%MEM`: Percentage of memory used.
- `TIME+`: Total CPU time used.

### Interactive Commands (while `top` is running):
- `k`: Kill a process (prompts for PID).
- `q`: Quit `top`.
- `M`: Sort by memory usage.
- `P`: Sort by CPU usage.

---

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






