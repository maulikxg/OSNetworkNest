## Allocating Memory for the Process 

Once the OS locates the executable file and verifies its format, it **allocates memory** for the process before execution. This step ensures that the process has the necessary memory resources to run.

---

## **🔹 1. Memory Segments of a Process**
When a process starts, the OS **divides memory into segments**:

| **Memory Segment** | **Purpose** | **Example Content** | **Growth Direction** |
|-----------------|------------|----------------|----------------|
| **Text (Code) Segment** | Stores the compiled program instructions (read-only). | The machine code of `main()` and functions. | Fixed Size |
| **Data Segment** | Stores global and static variables. | `int global_var = 10;` | Fixed Size |
| **BSS Segment** | Stores **uninitialized** global and static variables. | `static int uninitialized_var;` | Fixed Size |
| **Heap Segment** | Stores dynamically allocated memory (`malloc()`, `new`). | `int *ptr = malloc(sizeof(int));` | Expands **Upward** |
| **Stack Segment** | Stores local variables, function calls, return addresses. | `int local_var;` | Expands **Downward** |

📌 **Memory Layout Example:**  

```
+--------------------+  Higher Memory Addresses
| Stack             |  (Local variables, function calls)
|        ⇓          |
| Heap              |  (Dynamic memory allocation)
|        ⇑          |
| BSS Segment       |  (Uninitialized global/static vars)
| Data Segment      |  (Initialized global/static vars)
| Text Segment      |  (Program code)
+--------------------+  Lower Memory Addresses
```

---

## **🔹 2. How the OS Allocates Memory to a Process**
The OS uses **Virtual Memory Management (VMM)** to allocate memory to a process.

### **Step 1: Create a Virtual Address Space**
- Each process gets a **separate virtual address space**.
- This prevents **one process from accessing another’s memory**.
- The OS assigns **page tables** to map **virtual memory → physical memory**.

### **Step 2: Load the Text (Code) Segment**
- The OS **loads the program’s instructions** (machine code) from disk into **RAM**.
- It assigns a fixed amount of memory for the **text segment**.

Example:
```c
#include <stdio.h>

void hello() {
    printf("Hello, world!\n");
}

int main() {
    hello();
    return 0;
}
```
- The compiled binary contains **machine instructions** for `hello()` and `main()`, which go into the **text segment**.

### **Step 3: Allocate Data & BSS Segments**
- The OS reserves space for:
  - **Global and static variables (Data Segment)**
  - **Uninitialized variables (BSS Segment)**

Example:
```c
int global_var = 10;  // Data Segment
static int uninitialized_var;  // BSS Segment
```

### **Step 4: Set Up the Stack**
- The OS **allocates stack space** for function calls and local variables.
- When `main()` starts, the **stack frame** is created.

Example:
```c
int main() {
    int local_var = 20;  // Allocated on Stack
}
```

📌 **What Happens in Memory?**  
```
+--------------------+
| Stack             | (local_var = 20)
| Heap              | (empty, no malloc)
| BSS Segment       | (uninitialized_var)
| Data Segment      | (global_var = 10)
| Text Segment      | (machine code)
+--------------------+
```

### **Step 5: Initialize the Heap**
- The **heap starts empty** but can grow dynamically.

Example:
```c
int *ptr = malloc(sizeof(int) * 5); // Allocates 5 integers in Heap
```
Now, the **heap segment** expands **upward**.

---

## **🔹 3. How the OS Manages Memory Allocation?**
The OS **allocates and deallocates memory dynamically**:
1. **Paging** – Divides memory into fixed-size **pages** (4KB, 8KB).
2. **Segmentation** – Divides memory based on logical sections (**code, data, stack, heap**).
3. **Swapping** – Moves inactive pages to disk when RAM is full (swap space).

---

## **🔹 4. Real-World Examples**
🔹 **Check Process Memory Allocation in Linux**
```bash
cat /proc/<PID>/maps
```
Example Output & Explanation:
```
00400000-00452000 r-xp 00000000 08:01 123456 /bin/bash  # Text Segment (Executable Code)
00652000-00653000 r--p 00052000 08:01 123456 /bin/bash  # Data Segment (Initialized Data)
00653000-00654000 rw-p 00053000 08:01 123456 /bin/bash  # BSS Segment (Uninitialized Data)
7ffd2c3fa000-7ffd2c41b000 rw-p 00000000 00:00 0 [stack]  # Stack (Function Calls & Local Variables)
```

🔹 **Monitor Process Memory Usage**
```bash
top -p <PID>
```
Example Output & Explanation:
```
PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND  
1234 user     20   0   500M   20M   10M S   0.3  1.0   0:00.50 process_name
```
- **VIRT**: Virtual Memory used by the process.
- **RES**: Resident Memory (Actual RAM used).
- **SHR**: Shared Memory.
- **%MEM**: Percentage of total RAM used.

🔹 **Check Heap Memory Usage**
```bash
pmap <PID>
```
Example Output & Explanation:
```
1234:   process_name
0000000000400000    132K r-x-- process_name  # Text Segment
0000000000600000      4K r---- process_name  # Read-Only Data Segment
0000000000601000      4K rw--- process_name  # Writable Data Segment
00007f8a1b32d000    204K rw---   [heap]      # Heap Memory
```
- **[heap]** section shows the dynamically allocated memory.

---

## **🔹 Summary**
1. The OS **creates virtual memory space** for the process.
2. It **loads the program’s machine code** into the **text segment**.
3. It **allocates memory for global/static variables** (Data & BSS Segments).
4. It **sets up the stack** for function calls and local variables.
5. It **allocates the heap** dynamically when needed.
6. The OS **uses paging & swapping** to manage memory efficiently.

---
---

## Exploring `pmap` in Detail

The `pmap` command in Linux is used to display the **memory map of a process**, showing how memory is allocated and used. It helps analyze **memory usage**, **shared libraries**, **heap/stack size**, and **memory leaks**.

---

## 1️⃣ Basic Syntax
```bash
pmap [options] <PID>
```
Here, `<PID>` is the **Process ID** of the running program.

### Example
Find the PID of a running process (`firefox` in this case):
```bash
ps aux | grep firefox
```
Assume the PID is `1234`, then run:
```bash
pmap 1234
```

🔹 **Output Example:**
```
1234:   firefox
0000563e49a90000  276K r-xp /usr/bin/firefox
0000563e49d96000   12K r--p /usr/bin/firefox
00007f3a90a00000  1024K rw-p [heap]
00007f3a91200000  4096K r--p /usr/lib/x86_64-linux-gnu/libc.so.6
00007f3a91600000  4096K r-xp /usr/lib/x86_64-linux-gnu/libc.so.6
00007f3a91a00000  2048K rw-p [stack]
...
```

---

## 2️⃣ Understanding the `pmap` Output

| **Column**  | **Description** |
|------------|----------------|
| **Address**  | Virtual memory address of the allocated region |
| **Kbytes**   | Size of the memory region in kilobytes |
| **Mode**     | Access permissions (`r`=read, `w`=write, `x`=execute, `p`=private, `s`=shared) |
| **Mapping**  | Name of the file/library or `[heap]` / `[stack]` |

---

## 3️⃣ Common Options for `pmap`

### 🔹 1. Show Detailed Memory Usage (`-x`)
```bash
pmap -x 1234
```
🔹 **Example Output**
```
Address           Kbytes     RSS   Dirty Mode Mapping
0000563e49a90000  276       32    0      r-xp /usr/bin/firefox
00007f3a90a00000  1024      256   128    rw-p [heap]
```
**New Columns Explained:**
- **RSS (Resident Set Size):** Actual memory in RAM (not swapped out).
- **Dirty:** Modified memory pages.

---

### 🔹 2. Show Total Memory Usage (`-d`)
```bash
pmap -d 1234
```
🔹 **Example Output**
```
Address           Kbytes     Mode  Mapping
...
total Kbytes: 1400000
```
Displays the **total memory usage** of the process.

---
---

## 4️⃣ Analyzing Memory Usage

### **Heap (`[heap]`)**
- Shows dynamically allocated memory (e.g., `malloc()` in C).
- If it grows too large, the process might have **memory leaks**.

### **Stack (`[stack]`)**
- Holds function call frames, local variables.
- If it grows too large, it can cause a **stack overflow**.

### **Shared Libraries (`/usr/lib/...`)**
- Lists dynamically linked libraries used by the process.
- Shared libraries help save memory when used by multiple processes.

---

## 5️⃣ Finding Memory Leaks Using `pmap`
If a process consumes a lot of memory over time:
1. **Check initial memory usage**
   ```bash
   pmap -x <PID>
   ```
2. **Monitor it over time** (e.g., every 5 seconds)
   ```bash
   watch -n 5 "pmap -x <PID> | tail -1"
   ```
3. If RSS keeps increasing but Dirty stays low, the process **might have a memory leak**.

---

## 6️⃣ Summary of `pmap` Options

| **Option** | **Description** |
|-----------|----------------|
| `pmap <PID>` | Show memory map of a process |
| `pmap -x <PID>` | Show detailed memory usage (RSS, Dirty) |
| `pmap -d <PID>` | Show total memory usage |



---

### 🔹 Conclusion
- `pmap` helps **analyze memory usage** and **detect leaks**.
- `pmap -x` is useful for checking **actual memory in RAM**.
- Use `pmap -d` to see **total memory usage**.

Would you like an example with **real-time memory monitoring**? 🚀


---
---

### **What Happens to Memory When a Process Finishes?**

When a process finishes (either normally or abnormally), the operating system (OS) must **clean up** and **release** its allocated resources, including **memory**. Below are the key steps in this process:

---

## **🔹 1. Process Termination**
A process can finish in multiple ways:
- **Normal exit:** The process completes its execution and calls `exit()`.
- **Abnormal termination:** The process is forcefully killed due to an error, signal, or manual intervention (`kill` command).

Once terminated, the OS starts reclaiming its memory.

---

## **🔹 2. Freeing Virtual Memory**
Each process is assigned a **virtual address space**. When it ends:
- The OS **deallocates the virtual memory pages** assigned to the process.
- Any **heap memory (allocated via `malloc`, `new`) is freed**.
- The **stack memory (used for function calls and local variables) is released**.
- The **memory mapping (if any) is removed**.

---

## **🔹 3. Page Table Entries are Removed**
- The **page table** (which maps virtual memory to physical memory) is cleared.
- The **TLB (Translation Lookaside Buffer)** entries related to the process are invalidated.

This ensures that no part of the terminated process remains accessible.

---

## **🔹 4. Releasing Physical Memory (RAM)**
- The OS marks the **physical memory (RAM) pages** used by the process as **free**.
- These pages are returned to the **free memory pool**.
- The freed memory can now be allocated to **other processes**.

---

## **🔹 5. Closing File Descriptors & Freeing Kernel Resources**
If the process had opened **files, sockets, or pipes**, the OS:
- Closes all **file descriptors**.
- Releases **network connections** and **shared memory**.
- Removes **semaphores, locks, and other IPC resources**.

---

## **🔹 6. Removing Process Control Block (PCB)**
- The **PCB (Process Control Block)**, which holds process metadata (PID, state, registers, etc.), is deleted.
- The process is **removed from the process table**.

---

## **🔹 7. Zombie Processes (If Parent Doesn't Reap)**
- If a **parent process does not call `wait()`**, the process becomes a **zombie**.
- A **zombie** is an entry in the process table without memory but still waiting to be cleaned up.
- The OS removes it when the parent process reads its exit status.

---

## **🔹 Example Scenario**
Consider a program running this C code:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr = (int*) malloc(100 * sizeof(int)); // Allocating memory
    printf("Process running...\n");
    free(arr); // Freeing memory (optional, OS will do it)
    return 0;
}
```

### **What Happens When This Process Ends?**
- The **heap memory** (`malloc`) is released.
- The **stack (local variables) is deallocated**.
- The **page table entries are cleared**.
- The **process PCB is deleted**.
- The **memory used is returned to the system**.

---

## **🔹 Summary**
| **Step** | **What Happens?** |
|---------|------------------|
| **1. Process Ends** | Process exits normally or is terminated. |
| **2. Free Virtual Memory** | Heap, stack, and mapped memory are deallocated. |
| **3. Remove Page Tables** | The OS clears page table entries and TLB. |
| **4. Release Physical Memory** | RAM pages are marked as free. |
| **5. Close File Descriptors** | Open files, sockets, and IPC resources are released. |
| **6. Remove PCB** | The process is removed from the process table. |
| **7. Handle Zombies** | If the parent doesn’t reap it, it becomes a zombie. |
 🚀
