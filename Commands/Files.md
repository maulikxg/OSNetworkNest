# Linux File Commands

## Basic File Operations Commands

### 1. ls (List Directory Contents)
**Basic Syntax:** `ls [options] [directory]`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -l | Long listing format | `ls -l file.txt` | `-rw-r--r-- 1 user group 1234 Jan 31 12:00 file.txt` |
| -a | Show hidden files | `ls -a` | Shows `.hidden_file` along with regular files |
| -h | Human readable sizes | `ls -lh` | Shows sizes as 1K, 234M, 2G, etc. |
| -R | Recursive listing | `ls -R /dir` | Lists contents of dir and all subdirectories |
| -t | Sort by modification time | `ls -lt` | Most recently modified files first |
| -S | Sort by file size | `ls -lS` | Largest files first |
| -r | Reverse sort order | `ls -lr` | Reverses any sort order |
| --color | Colorize output | `ls --color=auto` | Shows files in different colors based on type |

### 2. cp (Copy)
**Basic Syntax:** `cp [options] source destination`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -r | Recursive copy | `cp -r dir1 dir2` | Copies dir1 and all contents to dir2 |
| -p | Preserve attributes | `cp -p file1 file2` | Maintains original timestamps, permissions |
| -i | Interactive | `cp -i file1 file2` | Prompts before overwrite |
| -v | Verbose | `cp -v file1 file2` | Shows detailed copy progress |
| -u | Update | `cp -u file1 file2` | Copies only when source is newer |
| -n | No overwrite | `cp -n file1 file2` | Never overwrites existing file |
| --backup | Create backups | `cp --backup file1 file2` | Creates backup of existing files |

### 3. mv (Move)
**Basic Syntax:** `mv [options] source destination`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -i | Interactive | `mv -i file1 file2` | Prompts before overwrite |
| -f | Force move | `mv -f file1 file2` | Overwrites without prompt |
| -n | No overwrite | `mv -n file1 file2` | Never overwrites existing file |
| -v | Verbose | `mv -v file1 file2` | Shows detailed move progress |
| -u | Update | `mv -u file1 file2` | Moves only when source is newer |
| -b | Backup | `mv -b file1 file2` | Creates backup of destination file |

### 4. rm (Remove)
**Basic Syntax:** `rm [options] file(s)`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -r | Recursive removal | `rm -r dir1` | Removes directory and contents |
| -f | Force removal | `rm -f file1` | No prompt, ignore nonexistent files |
| -i | Interactive | `rm -i file1` | Prompt before every removal |
| -v | Verbose | `rm -v file1` | Shows detailed deletion progress |
| -d | Remove empty directories | `rm -d dir1` | Removes only if directory is empty |
| --preserve-root | Protect root | `rm --preserve-root /` | Prevents accidental root deletion |

### 5. find
**Basic Syntax:** `find [path] [options] [expression]`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -name | Search by name | `find . -name "*.txt"` | Finds all .txt files |
| -type | Search by type | `find . -type d` | Finds directories only |
| -mtime | Modified time | `find . -mtime +30` | Files modified more than 30 days ago |
| -size | Search by size | `find . -size +1M` | Files larger than 1 MB |
| -exec | Execute command | `find . -name "*.tmp" -exec rm {} \;` | Removes all .tmp files |
| -user | Search by owner | `find . -user john` | Files owned by user john |
| -perm | Search by permission | `find . -perm 644` | Files with permission 644 |

### 6. chmod (Change Mode)
**Basic Syntax:** `chmod [options] mode file(s)`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -R | Recursive | `chmod -R 755 dir/` | Changes permissions recursively |
| -v | Verbose | `chmod -v 644 file` | Shows all changes made |
| -f | Force | `chmod -f 777 file` | Suppress error messages |
| --reference | Copy permissions | `chmod --reference=file1 file2` | Copies file1's permissions to file2 |
| -c | Report changes | `chmod -c 644 file` | Reports only when changes made |

### 7. chown (Change Owner)
**Basic Syntax:** `chown [options] user[:group] file(s)`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -R | Recursive | `chown -R user:group dir/` | Changes ownership recursively |
| -v | Verbose | `chown -v user file` | Shows all changes made |
| -f | Force | `chown -f user file` | Suppress error messages |
| --reference | Copy ownership | `chown --reference=file1 file2` | Copies file1's ownership to file2 |
| -h | Symbolic links | `chown -h user link` | Change link not target |

### 8. tar (Tape Archive)
**Basic Syntax:** `tar [options] [archive-file] [file(s)]`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -c | Create archive | `tar -cf archive.tar files/` | Creates new archive |
| -x | Extract archive | `tar -xf archive.tar` | Extracts archive |
| -v | Verbose | `tar -cvf archive.tar files/` | Shows progress |
| -z | Use gzip | `tar -czf archive.tar.gz files/` | Creates compressed archive |
| -j | Use bzip2 | `tar -cjf archive.tar.bz2 files/` | Creates bzip2 compressed archive |
| -t | List contents | `tar -tf archive.tar` | Lists archive contents |
| --exclude | Exclude pattern | `tar -cf archive.tar --exclude='*.tmp' files/` | Excludes .tmp files |

### 9. grep (Global Regular Expression Print)
**Basic Syntax:** `grep [options] pattern [file(s)]`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -i | Case insensitive | `grep -i "text" file` | Matches TEXT, text, Text |
| -r | Recursive | `grep -r "pattern" dir/` | Searches all files in directory |
| -l | List files | `grep -l "pattern" *` | Shows only filenames with matches |
| -n | Line numbers | `grep -n "pattern" file` | Shows matching line numbers |
| -v | Invert match | `grep -v "pattern" file` | Shows non-matching lines |
| -w | Match words | `grep -w "word" file` | Matches only whole words |
| -c | Count matches | `grep -c "pattern" file` | Shows count of matching lines |

### 10. du (Disk Usage)
**Basic Syntax:** `du [options] [file(s)]`

| Flag | Description | Example | Output/Result |
|------|-------------|---------|---------------|
| -h | Human readable | `du -h file` | Shows sizes in K, M, G |
| -s | Summarize | `du -s dir/` | Shows only total for directory |
| -a | All files | `du -a dir/` | Shows size for all files |
| -c | Total | `du -c dir/` | Shows total after listing |
| --max-depth | Depth limit | `du --max-depth=1 dir/` | Shows usage one level deep |
| --exclude | Exclude pattern | `du --exclude='*.tmp' dir/` | Excludes .tmp files |
| -t | Threshold | `du -t 10M dir/` | Shows files over 10MB |

