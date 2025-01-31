# Linux File Operations Cheat Sheet

## 1. File Management
### Create, List, View Files
- **Creating an Empty File:** Use `touch` to create an empty file. Useful for placeholders or quick testing.
```bash
touch filename  # Create an empty file
```
- **Listing Files in a Directory:**
```bash
ls -l           # List files with details (permissions, owner, size, timestamp)
ls -a           # Show hidden files (files starting with .)
```
- **Viewing File Contents:** Use these commands when you need to inspect file content.
```bash
cat filename    # Display the whole file at once
less filename   # View file with scroll (recommended for large files)
head -n 10 filename  # Show first 10 lines of the file
tail -n 10 filename  # Show last 10 lines of the file
```

## 2. Copy, Move, Delete Files
- **Copying Files and Directories:**
```bash
cp source dest      # Copy a file
cp -r dir1 dir2     # Copy a directory (recursively)
```
- **Renaming or Moving Files:**
```bash
mv old new          # Rename or move file to another location
```
- **Deleting Files and Directories:**
```bash
rm filename        # Delete a single file
rm -r directory    # Delete directory and all its contents (use with caution)
```

## 3. File Compression
- **Creating and Extracting Archives:** Useful for backup or transferring multiple files.
```bash
tar -cvf archive.tar file1 file2  # Create tar archive
tar -xvf archive.tar              # Extract tar archive
```
- **Compressing Files:** Reduces file size for storage efficiency.
```bash
gzip filename                     # Compress file (creates filename.gz)
gunzip filename.gz                # Decompress gzip file
tar -czvf archive.tar.gz file1    # Create tar.gz (compressed archive)
tar -xvzf archive.tar.gz          # Extract tar.gz
```

## 4. File Permissions
- **Checking Permissions:**
```bash
ls -l filename    # View file permissions
```
- **Changing File Permissions:** Use `chmod` when you need to grant or restrict access.
```bash
chmod 755 file    # Owner has full access; others can read and execute
chmod -R 755 dir  # Apply the same permission recursively to a directory
```
- **Changing Ownership:** Useful for fixing permission issues.
```bash
chown user:group filename  # Change file owner and group
```

## 5. Find & Search Files
- **Finding Files by Name or Type:**
```bash
find /path -name "filename"  # Find a file by its name
find /path -type f -name "*.txt"  # Find all .txt files in a directory
```
- **Searching Inside Files:** Helps locate specific content within files.
```bash
grep "pattern" filename       # Search for a pattern inside a file
grep -r "pattern" directory   # Recursively search inside all files in a directory
```

## 6. Symbolic & Hard Links
- **Creating a Symbolic Link (Shortcut):**
```bash
ln -s /path/to/file linkname  # Create a symbolic link (shortcut)
```
- **Creating a Hard Link (Duplicate File Reference):**
```bash
ln /path/to/file hardlinkname # Create a hard link
```

## 7. Disk Usage & File Size
- **Checking Disk Usage:** Useful for identifying large files and monitoring storage.
```bash
du -sh filename   # Show file size in human-readable format
du -sh directory  # Show total directory size
df -h             # Show available and used disk space on all mounted filesystems
```

## 8. Editing Files
- **Using Text Editors:** Modify file contents easily.
```bash
nano filename  # Open file in nano editor (beginner-friendly)
vim filename   # Open file in Vim editor (advanced users)
```

## 9. File Integrity
- **Verifying File Integrity:** Ensure data integrity during transfers.
```bash
md5sum filename      # Generate MD5 checksum
sha256sum filename   # Generate SHA-256 checksum (more secure than MD5)
```

---
### 🔹 **Usage Scenarios:**
- **Use `find` when tracking down misplaced files.**
- **Use `grep` when searching for a specific string inside log files.**
- **Use `chmod` and `chown` when setting up file permissions on a server.**
- **Use `tar` and `gzip` when backing up important directories.**
- **Use `md5sum` to ensure file integrity after downloading from the internet.**
