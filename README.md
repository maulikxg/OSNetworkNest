# Basics of Operating System - Linux Commands

## Overview
The operating system is system software that manages hardware resources like RAM, memory, CPU, and software resources like processes, threads, system libraries, networking protocols, and user management. It provides a platform on top of which other programs (applications) can run.

---

## Types of Operating Systems

### 1. **Batch Operating System**
   - Operator takes similar jobs having the same requirements and groups them into batches.

### 2. **Multiprogramming**
   - More than one program is present in the main memory, and any one of them can be kept in execution.

### 3. **Time Sharing**
   - A type of multiprogramming system where every process runs in a round-robin manner. Each task is given some time to execute so that all the tasks work smoothly.

### 4. **Multi-tasking**
   - Time-sharing and multiprogramming systems are also called multitasking, as multiple tasks run in an interleaving manner.

### 5. **Multi-processing**
   - More than one CPU is used for the execution of resources.

### 6. **Multi-user**
   - These systems allow multiple users to be active at the same time. These systems can be either multiprocessor or single processor with interleaving.

### 7. **Distributed**
   - Interconnected computers communicate with each other using a shared communication network. Independent systems possess their own memory unit and CPU.

### 8. **Network**
   - These systems run on a server and provide the capability to manage data, users, groups, security, applications, and other networking functions.

### 9. **Real-time**
   - Real-time systems are used when there are very strict time requirements, such as missile systems, air traffic control systems, robots, etc.

---

## What is Kernel?
- The kernel acts as a bridge between hardware resources and software.
- It manages the communication of system resources with software applications.

  ## Kernel Types

| Kernel Type       | Description | Working | Key Characteristics | Example |
|------------------|-------------|---------|----------------------|---------|
| **Simple Kernel** | A basic, minimalistic kernel for small systems or embedded environments. | Directly interacts with hardware and handles only essential tasks. No multitasking or advanced features. | Minimal functionality, direct hardware access, limited scope. | Embedded systems |
| **Monolithic Kernel** | A large kernel where all system services are bundled together. | All services (process management, memory, device drivers, etc.) run in kernel space, giving fast communication. | All services in kernel space, high performance, tightly coupled. | Linux, Unix |
| **Micro-Kernel** | A minimal kernel that handles only essential tasks, with other services in user space. | The kernel provides core functionalities like scheduling, and other services run as separate processes in user space. | Small kernel, modular, slower communication between kernel and user space. | Minix, QNX |
| **Hybrid-Kernel** | Combines elements of both microkernel and monolithic kernel architectures. | Some services run in kernel space for performance, while others run in user space for modularity. | Combination of microkernel features, some services in kernel space. | Windows NT, macOS |
| **Exo-Kernel** | A minimal kernel that provides basic hardware abstraction, leaving resource management to applications. | Exposes low-level hardware resources to applications, which manage them directly. | Minimal hardware abstraction, high flexibility, apps manage resources. | Aegis (research) |
| **Layered Kernel** | A kernel organized into distinct layers, with each responsible for specific functions. | Each layer interacts only with the layer directly above or below it, providing separation of concerns. | Structured into layers, modular, clear separation of concerns. | OSI model (networking) |
| **Modular Kernel** | A kernel with independent modules that can be loaded and unloaded as needed. | Allows dynamic loading and unloading of components, like drivers, while the system is running. | Flexible, modular, dynamic loading of components, less tightly coupled. | Linux (with modules) |

