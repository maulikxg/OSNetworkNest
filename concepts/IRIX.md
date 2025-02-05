# **IRIX Mode in the `top` Command (Detailed Explanation)**

## **📌 What is IRIX Mode?**
IRIX mode in the `top` command controls **how CPU usage is displayed** in the `%CPU` column. It determines whether the CPU usage of a process is displayed as:

1. **IRIX Mode ON (Default)**
   - CPU usage is **not divided by the number of CPU cores**.
   - A single process **can show more than 100% CPU usage** if it's using multiple cores.
   - Total CPU usage across processes **can exceed 100%** on multi-core systems.

2. **IRIX Mode OFF**
   - CPU usage **is divided by the number of CPU cores**.
   - A process using multiple cores will **not exceed 100%**.
   - Total CPU usage across all processes is limited to **100%**.

---

## **🔹 Impact of IRIX Mode on CPU Display**

### **🟢 IRIX Mode ON (Default)**
- Assume a system with **4 CPU cores**.
- A process using all 4 cores can show **up to 400% CPU usage**.

```
  PID USER      %CPU  COMMAND
 1234 root      250.0  python
 2345 user1     120.0  java
 3456 user2      80.0  nginx
```
✅ **Useful for multi-core performance analysis.**

---

### **🔴 IRIX Mode OFF**
- The same process will show CPU usage **as a fraction of the total CPU power**.

```
  PID USER      %CPU  COMMAND
 1234 root       62.5  python
 2345 user1      30.0  java
 3456 user2      20.0  nginx
```
✅ **Useful for comparing total CPU utilization across processes.**

---

## **🔹 How to Toggle IRIX Mode in `top`?**

### **Method 1: Interactive Mode (Inside `top`)**
1. Open `top`:
   ```bash
   top
   ```
2. Press **Shift + I** (**capital 'i'**) to toggle IRIX mode ON/OFF.

### **Method 2: Using Command Line Option**
- Run `top` in **IRIX mode OFF**:
  ```bash
  top -S
  ```
- Run `top` in **IRIX mode ON**:
  ```bash
  top
  ```

---

## **🔹 When Should You Use IRIX Mode?**

| **Scenario** | **IRIX Mode ON** | **IRIX Mode OFF** |
|-------------|----------------|----------------|
| Multi-core performance analysis | ✅ Yes | ❌ No |
| CPU usage comparison across processes | ❌ No | ✅ Yes |
| Understanding per-core CPU load | ✅ Yes | ❌ No |
| Monitoring CPU usage on a single-core system | ❌ No | ✅ Yes |

---

## **🔹 Summary**
- **IRIX Mode ON (Default)** → Displays CPU usage **without dividing by CPU cores** (can exceed 100%).
- **IRIX Mode OFF** → CPU usage is **normalized per CPU core** (total max is 100%).
- Toggle it using **Shift + I** inside `top`.



