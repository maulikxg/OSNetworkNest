
---

# **File Transfer Commands and Protocols**

## **1. Introduction**
File transfer operations are essential for moving files between local and remote systems. Various tools and protocols can be used, depending on security and efficiency requirements.

---

## **2. File Transfer Commands**

### **2.1 `rsync` (File Synchronization)**
#### **Usage:**
- Efficient for local and remote file transfers.
- Supports incremental synchronization.

#### **Syntax:**
```bash
rsync [options] source destination
```
#### **Common Flags:**
- `-r` → Recursive copy.
- `-a` → Archive mode (preserves attributes).
- `-z` → Compress data.
- `--progress` → Show progress.
- `--delete` → Remove extra files at the destination.
- `-v` → Verbose output.

#### **Example:**
- **Copying files from local to remote with progress display:**
```bash
rsync -avz --progress source/ user@remote:/path/to/destination/
```
- **Synchronize and delete extraneous files from the destination:**
```bash
rsync -avz --delete source/ user@remote:/path/to/destination/
```

---

### **2.2 `scp` (Secure Copy Protocol)**
#### **Usage:**
- Securely copies files over SSH.

#### **Syntax:**
```bash
scp [options] source destination
```
#### **Common Flags:**
- `-r` → Copy directories recursively.
- `-C` → Enable compression.
- `-v` → Show debug information.
- `-P <port>` → Specify SSH port.
- `-i <identity_file>` → Use a specific private key for authentication.

#### **Examples:**
- **Copy file from local to remote:**
```bash
scp file.txt user@remote:/path/to/destination/
```
- **Copy file from remote to local:**
```bash
scp user@remote:/path/to/file.txt /local/destination/
```
- **Copy directory recursively with a custom SSH port:**
```bash
scp -r -P 2222 myfolder user@remote:/path/to/destination/
```
- **Use a specific SSH key for authentication:**
```bash
scp -i ~/.ssh/mykey.pem file.txt user@remote:/path/to/destination/
```

---

### **2.3 `sftp` (Secure File Transfer Protocol)**
#### **Usage:**
- Transfers files interactively over SSH.

#### **Syntax:**
```bash
sftp user@remote
```
#### **Common Commands:**
- `put <file>` → Upload file.
- `get <file>` → Download file.
- `ls` → List remote files.
- `lcd <directory>` → Change local directory.
- `cd <directory>` → Change remote directory.
- `mput <files>` → Upload multiple files.
- `mget <files>` → Download multiple files.
- `exit` / `bye` → Close connection.

#### **Example:**
- **Connect to remote server:**
```bash
sftp user@remote
```
- **Upload a file:**
```bash
put mydata.txt
```
- **Download a file:**
```bash
get report.pdf
```
- **Transfer directory recursively:**
```bash
put -r myfolder
```
- **Upload multiple files:**
```bash
mput *.txt
```

---

### **2.4 `wget` (Download from Web)**
#### **Usage:**
- Downloads files from HTTP/HTTPS/FTP.

#### **Syntax:**
```bash
wget [options] URL
```
#### **Common Flags:**
- `-c` → Resume partial download.
- `-O <filename>` → Save as a different filename.
- `-r` → Download recursively.
- `-np` → Prevent ascending to parent directories.
- `-A <extensions>` → Accept files with specific extensions.

#### **Example:**
- **Resume download of a file:**
```bash
wget -c -O newfile.html https://example.com/file.html
```
- **Download a website recursively (without ascending directories):**
```bash
wget -r -np https://example.com/
```
- **Download specific file types (e.g., only `.jpg` images):**
```bash
wget -r -A jpg https://example.com/images/
```

---

### **2.5 `curl` (Download & Upload via HTTP/FTP)**
#### **Usage:**
- Downloads and uploads data.

#### **Syntax:**
```bash
curl [options] URL
```
#### **Common Flags:**
- `-o <filename>` → Save as a different filename.
- `-C -` → Resume download.
- `-I` → Fetch headers only.
- `-L` → Follow redirects.
- `-F` → Send data with `POST` method.

#### **Example:**
- **Download a file and save with a different name:**
```bash
curl -o newfile.zip https://example.com/file.zip
```
- **Resume download:**
```bash
curl -C - -O https://example.com/file.zip
```
- **Follow redirects (e.g., in case of a URL redirection):**
```bash
curl -L -O https://example.com/redirectedfile.zip
```

---

## **3. Network Ports Used in File Transfers**
| Protocol  | Port(s)  | Description |
|-----------|---------|-------------|
| **SSH**   | 22      | Secure remote login & file transfer |
| **Telnet** | 23      | Insecure remote login (deprecated) |
| **FTP**   | 21 (control), 20 (data) | Traditional file transfer (insecure) |
| **SFTP**  | 22      | Secure file transfer over SSH |
| **HTTP**  | 80      | Web file transfers (insecure) |
| **HTTPS** | 443     | Secure web file transfers |

### **Comparison of File Transfer Protocols**
| Protocol  | Security | Speed | Use Case |
|-----------|---------|-------|----------|
| **SSH (SCP/SFTP)** | Encrypted (secure) | Fast | Remote file transfer |
| **Telnet** | Insecure | Moderate | Remote login (not recommended) |
| **FTP** | Insecure | Fast | Traditional file transfer (requires authentication) |
| **SFTP** | Encrypted (secure) | Slightly slower than FTP | Secure file transfer over SSH |
| **HTTP** | Insecure | Fast | Downloading web resources |
| **HTTPS** | Encrypted (secure) | Fast | Secure web file downloads |

---



