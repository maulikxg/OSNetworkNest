# System Activity Report (`sar`)

The `sar` command is a powerful tool used to collect, report, and save system activity information. It helps monitor various system resources such as CPU utilization, memory usage, disk activity, and network performance over time.

## Commonly Used Flags

| Flag     | Description                                                                   |
| -------- | ----------------------------------------------------------------------------- |
| `-u`     | Displays CPU utilization report.                                              |
| `-r`     | Reports memory utilization (used, free, swap, etc.).                          |
| `-d`     | Shows device (disk) activity, including reads, writes, and I/O time.          |
| `-n`     | Used for network-related reports like interface statistics.                   |
| `-q`     | Displays queue length, processes in the run queue, and other process metrics. |
| `-B`     | Reports swap space utilization.                                               |
| `-P ALL` | Reports CPU statistics for all processors (when using `-u` flag).             |
| `-s`     | Specifies the interval in seconds between each report.                        |
| `-e`     | Allows you to specify the start and end time for the report.                  |
| `-f`     | Collects data in the specified file format.                                   |
| `-h`     | Displays help with usage information.                                         |

## Usage Examples

1. **Check CPU Utilization:**

   ```sh
   sar -u 5 10
   ```

   This command reports CPU usage every 5 seconds for 10 intervals.

2. **Check Memory Usage:**

   ```sh
   sar -r 5 5
   ```

   Displays memory statistics every 5 seconds for 5 intervals.

3. **Monitor Disk Activity:**

   ```sh
   sar -d 1 3
   ```

   Reports disk I/O every 1 second for 3 intervals.

4. **Get Network Interface Statistics:**

   ```sh
   sar -n DEV 3 4
   ```

   Displays network statistics every 3 seconds for 4 intervals.

5. **Specify Time Range:**

   ```sh
   sar -u -s 12:00:00 -e 13:00:00
   ```

   Reports CPU usage between 12:00 PM and 1:00 PM.

6. **View Swap Space Usage:**

   ```sh
   sar -B 5 5
   ```

   Reports swap usage every 5 seconds for 5 intervals.

The `sar` command is highly useful for performance monitoring and troubleshooting system resource issues. It provides valuable insights that help system administrators optimize and manage system performance efficiently.

