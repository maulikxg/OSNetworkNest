
---

# **10. Selection and Sorting Commands**

These commands are used to filter, process, and organize textual data or command output. They help you extract meaningful information from files or streams.

## **10.1 `grep` – Pattern Searching**

- **Usage:** Searches for lines in a file (or stream) that match a specified pattern.
- **Syntax:**
  ```bash
  grep [options] pattern [file...]
  ```
- **Common Flags and Examples:**
  - **`-i` (Ignore Case):**  
    Search without case sensitivity.
    ```bash
    grep -i "error" /var/log/syslog
    ```
  - **`-v` (Invert Match):**  
    Select lines that do **not** match the pattern.
    ```bash
    grep -v "success" logfile.txt
    ```
  - **`-r` (Recursive):**  
    Search files recursively in a directory.
    ```bash
    grep -r "function" /path/to/code
    ```
  - **`-n` (Line Numbers):**  
    Show line numbers along with matching lines.
    ```bash
    grep -n "ERROR" application.log
    ```

**Explanation of Output Metrics:**
- **Matched Lines:** Only lines containing (or not containing, with `-v`) the specified pattern are shown.
- **Line Numbers:** When using `-n`, each matching line is prefixed with its line number from the file.
- **Case Insensitivity:** The `-i` flag ensures that matches occur regardless of letter case.

---

## **10.2 `awk` – Field Processing and Pattern Matching**

- **Usage:** Scans input for patterns and performs actions on matching lines. Particularly useful for column-based data manipulation.
- **Syntax:**
  ```bash
  awk 'pattern { action }' [file...]
  ```
- **Common Examples and Flags:**
  - **Print a Specific Field:**  
    Print the first field (typically the username) from each line of `/etc/passwd`.
    ```bash
    awk '{print $1}' /etc/passwd
    ```
  - **Field Separator with `-F`:**  
    Use a custom field separator (e.g., colon `:`).
    ```bash
    awk -F: '{print $1 " has UID " $3}' /etc/passwd
    ```
  - **Conditional Processing:**  
    Print lines where the third field is greater than 1000.
    ```bash
    awk '$3 > 1000 {print $0}' /etc/passwd
    ```

**Explanation of Output Metrics:**
- **Field Extraction:** The command outputs specified columns from each line.
- **Conditional Output:** Only lines meeting the condition (if any) are printed.
- **Custom Field Separators:** The `-F` flag allows you to define which character separates fields.

---

## **10.3 `sort` – Sorting Lines**

- **Usage:** Sorts lines of text from a file or stream.
- **Syntax:**
  ```bash
  sort [options] [file...]
  ```
- **Common Flags and Examples:**
  - **`-n` (Numeric Sort):**  
    Sort lines numerically.
    ```bash
    sort -n numbers.txt
    ```
  - **`-r` (Reverse):**  
    Reverse the sort order.
    ```bash
    sort -r names.txt
    ```
  - **`-u` (Unique):**  
    Output only unique lines after sorting.
    ```bash
    sort names.txt | uniq
    ```

**Explanation of Output Metrics:**
- **Ordered Output:** Lines are arranged based on the sort key (numerically or lexicographically).
- **Unique Lines:** When combined with `uniq`, repeated lines are filtered out.
- **Reverse Order:** The `-r` flag inverts the default order.

---

## **10.4 `uniq` – Filtering Duplicate Lines**

- **Usage:** Filters out or reports adjacent duplicate lines.
- **Syntax:**
  ```bash
  uniq [options] [input_file [output_file]]
  ```
- **Common Flags and Examples:**
  - **`-c` (Count Occurrences):**  
    Prefix each output line with the number of occurrences.
    ```bash
    sort names.txt | uniq -c
    ```
  - **`-d` (Display Duplicates Only):**  
    Show only the lines that are repeated.
    ```bash
    sort names.txt | uniq -d
    ```
  - **`-u` (Unique Lines Only):**  
    Display only unique lines (those that are not repeated).
    ```bash
    sort names.txt | uniq -u
    ```

**Explanation of Output Metrics:**
- **Frequency Counts:** The `-c` flag shows how many times each line appears.
- **Filtered Results:** Using `-d` or `-u` gives you a filtered view based on duplicates.

---

## **10.5 `head` and `tail` – Viewing File Portions**

- **Usage:** Displays the beginning (`head`) or the end (`tail`) of files.
- **Syntax:**
  ```bash
  head [options] [file...]
  tail [options] [file...]
  ```
- **Common Flags and Examples:**
  - **`-n <number>`:**  
    Specifies the number of lines to display.
    ```bash
    head -n 5 data.txt    # First 5 lines
    tail -n 5 data.txt    # Last 5 lines
    ```
  - **`-c <number>` (Bytes):**  
    Display a specific number of bytes.
    ```bash
    head -c 100 data.txt   # First 100 bytes of the file
    ```

**Explanation of Output Metrics:**
- **Line or Byte Count:** You control how many lines or bytes are shown.
- **File Sections:** Easily inspect either the start or end of large files.

---

# **11. Port Reachability Commands**

These commands help determine if a particular network port on a remote host is accessible, which is useful for troubleshooting network connectivity and verifying service availability.

## **11.1 `telnet`**

- **Usage:** Connects to a remote host on a specific port to test if that port is open.
- **Syntax:**
  ```bash
  telnet <hostname_or_IP> <port>
  ```
- **Common Flags and Examples:**
  - **Basic Connection Test:**
    ```bash
    telnet example.com 80
    ```
    **Explanation:**  
    Attempts to open a connection to `example.com` on port `80` (HTTP).  
    **Output Metrics:**
    - **Successful Connection:** You might see a blank screen or a banner message indicating the connection is open.
    - **Failure Messages:** Errors like "Connection refused" or timeouts indicate that the port is closed or unreachable.

---

## **11.2 `nc` (Netcat)**

- **Usage:** A versatile networking utility used to check port connectivity, transfer data, or act as a simple server/client.
- **Syntax:**
  ```bash
  nc [options] <hostname_or_IP> <port>
  ```
- **Common Flags and Examples:**
  - **`-v` (Verbose):**  
    Provides detailed output.
  - **`-z` (Zero-I/O Mode):**  
    Used for scanning without sending any data.
    ```bash
    nc -vz example.com 443
    ```
    **Explanation:**  
    Checks if port `443` (HTTPS) on `example.com` is open.  
    **Output Metrics:**
    - **Success Message:**  
      ```
      Connection to example.com 443 port [tcp/https] succeeded!
      ```
      Indicates that the port is reachable.
    - **Failure Message:**  
      ```
      nc: connect to example.com port 443 (tcp) failed: Connection refused
      ```
      Indicates that the port is not accessible.


---

# **Conclusion**

This document provided an overview of essential **Selection and Sorting Commands** (such as `grep`, `awk`, `sort`, `uniq`, `head`, and `tail`) along with their flags and example usages to help extract and organize data. It also detailed **Port Reachability Commands** (`telnet`, `nc`, and `nmap`), including their common flags, usage examples, and explanations of their output metrics. These tools are invaluable for troubleshooting, data processing, and network diagnostics.
