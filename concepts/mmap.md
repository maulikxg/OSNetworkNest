# Exploring mmap in Detail

## Introduction
Memory mapping (`mmap`) is a powerful mechanism that allows files or devices to be mapped into memory, enabling fast access without explicit read/write system calls. It is widely used for inter-process communication (IPC), large file handling, and performance optimizations.

## How mmap Works
`mmap` maps a file or device into the address space of a process. This allows the process to access the file as if it were part of memory.

### Syntax
```c
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
```

### Explanation of Parameters
1. **`addr` (void *)**: The starting address where the mapping should begin. If set to `NULL`, the kernel chooses an appropriate address.
2. **`length` (size_t)**: The number of bytes to map. This should be aligned to the system's page size (typically 4KB).
3. **`prot` (int)**: Memory protection settings, which control how the mapped memory can be accessed:
   - `PROT_READ`: Read permission.
   - `PROT_WRITE`: Write permission.
   - `PROT_EXEC`: Execute permission.
   - `PROT_NONE`: No access allowed.
   - These can be combined using bitwise OR (`|`), e.g., `PROT_READ | PROT_WRITE`.
4. **`flags` (int)**: Specifies the mapping type:
   - `MAP_SHARED`: Changes to the mapped memory are visible to other processes mapping the same file.
   - `MAP_PRIVATE`: Changes are not shared; modifications are private to the process.
   - `MAP_ANONYMOUS`: Mapping not backed by any file; used for allocating memory.
   - `MAP_FIXED`: Forces the mapping to be placed at the exact address specified in `addr` (dangerous, use cautiously).
   - `MAP_HUGETLB`: Uses large memory pages for better performance on supported systems.
5. **`fd` (int)**: File descriptor of the file to be mapped. If using `MAP_ANONYMOUS`, this should be set to `-1`.
6. **`offset` (off_t)**: The starting point in the file from where mapping should begin. Must be a multiple of the page size.

### Example Usage
```c
#include <fcntl.h>
#include <sys/mman.h>
#include <stdio.h>
#include <unistd.h>

int main() {
    int fd = open("test.txt", O_RDWR);
    if (fd == -1) {
        perror("open");
        return 1;
    }

    char *mapped = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (mapped == MAP_FAILED) {
        perror("mmap");
        return 1;
    }

    printf("Mapped Content: %s\n", mapped);
    munmap(mapped, 4096);
    close(fd);
    return 0;
}
```

## Understanding mmap Output Metrics
When using `mmap`, certain metrics provide insights into its behavior:

1. **Virtual Address**: The address range mapped into the process’s address space.
2. **Page Size**: Memory is mapped in page-sized chunks (commonly 4KB).
3. **Resident Set Size (RSS)**: Indicates how much of the mapping is loaded into RAM.
4. **Shared vs. Private Pages**:
   - Shared (`MAP_SHARED`): Changes affect all processes mapping the same file.
   - Private (`MAP_PRIVATE`): Changes are local to the process.
5. **Memory Protection (`prot`)**:
   - `PROT_READ`: Readable memory.
   - `PROT_WRITE`: Writable memory.
   - `PROT_EXEC`: Executable memory.
6. **Mapping Type (`flags`)**:
   - `MAP_ANONYMOUS`: Maps memory not backed by a file.
   - `MAP_FIXED`: Enforces mapping at a fixed address.
   - `MAP_HUGETLB`: Uses huge pages for performance.

## Real-Life Analogy
Imagine `mmap` as a **library**:
- **Books (files)** are stored in a **central archive (disk)**.
- Instead of manually fetching books (using `read()` system calls), you have a **desk (virtual memory)** where frequently used books are placed.
- You can **read and write** notes in these books (`PROT_READ | PROT_WRITE`).
- Some books are **shared (MAP_SHARED)**, so updates are visible to others.
- Others are **private (MAP_PRIVATE)**, so notes remain personal.
- Your **desk size (page size)** determines how many books you can have at once.
- The library **fetches missing books (page faults)** when needed.


