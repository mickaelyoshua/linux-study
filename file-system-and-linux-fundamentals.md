# Comprehensive Linux Command Reference

This document consolidates key Linux file system concepts, command‑line tools, and administration techniques into one reference. Each section follows a consistent format: **command name / topic**, purpose, useful options, examples, and extra information where applicable.

---

# Major Parts of Linux Directory Tree

1. Root `/`
	- Starting point of the filesystem.
	- Contains essential files for system boot and recovery.
	- Not to be confused with `/root` (root user’s home).

2. `/bin` — User Binaries
	- Essential executable programs for all users.
	- Example: `ls`, `cp`, `grep`.

3. `/sbin` — System Binaries
	- System maintenance executables.
	- Typically used by administrators.
	- Example: `fdisk`, `reboot`.

4. `/etc` — Configuration Files
	- System-wide configuration files.
	- Startup/shutdown scripts.
	- Example: `/etc/resolv.conf`.

5. `/dev` — Device Files
	- Interface to system devices.
	- Example: `/dev/tty1`.

6. `/proc` — Process Information
	- Virtual filesystem showing runtime system information.
	- Example: `/proc/cpuinfo`.

7. `/var` — Variable Files
	- Data expected to change over time.
	- Example: `/var/log`, `/var/mail`.

8. `/tmp` — Temporary Files
	- For temporary data; cleared on reboot.

9. `/usr` — User Programs
	- Non-essential user programs, libraries, documentation.
	- Subdirectories:
	- `/usr/bin`: User binaries.
	- `/usr/sbin`: Admin binaries.
	- `/usr/lib`: Libraries.
	- `/usr/local`: Locally compiled apps.

10. `/home` — User Home Directories
	- Personal user data.
	- Example: `/home/alice`.

11. `/boot` — Boot Loader Files
	- Contains kernel and bootloader.
	- Example: `vmlinuz`, `initrd`.

12. `/lib` — System Libraries
	- Essential libraries for binaries in `/bin` and `/sbin`.
	- Example: `libc.so`.

13. `/opt` — Optional Add-on Applications
	- For third-party or vendor software.

14. `/mnt` — Mount Directory
	- Temporary mounting of filesystems by sysadmins.

15. `/media` — Removable Media
	- Mount points for devices like CD-ROMs or USBs.
	- Example: `/media/usb`.

16. `/srv` — Service Data
	- Data for services hosted on the system.
	- Example: `/srv/ftp`.

---

## 2. Linux File Types

# Linux File Types and Their Descriptions
In Linux, everything is treated as a file, including hardware devices and inter-process communication mechanisms. Understanding the various file types is essential for effective system administration and scripting.

## 1. Regular File
- **Symbol**:	`-`
- **Description**:	Standard files that contain data, text, or program instructions. These are the most common type of files in Linux.
- **Examples**:	Text files, executable binaries, images, videos.

## 2. Directory
- **Symbol**:	`d`
- **Description**:	Files that contain references to other files and directories. They organize the filesystem hierarchy.
- **Examples**:	`/home`, `/etc`, `/var`.

## 3. Symbolic Link (Symlink)
- **Symbol**:	`l`
- **Description**:	A shortcut or reference to another file or directory. Useful for creating pointers or aliases.
- **Examples**:	`/usr/bin/python -> /usr/bin/python3.8`.

## 4. Character Device File
- **Symbol**:	`c`
- **Description**:	Represents devices that handle data as a stream of bytes (character by character), such as keyboards or serial ports.
- **Examples**:	`/dev/tty`, `/dev/random`.

## 5. Block Device File
- **Symbol**:	`b`
- **Description**:	Represents devices that handle data in blocks, such as hard drives or USB drives.
- **Examples**:	`/dev/sda`, `/dev/sdb`.

## 6. Named Pipe (FIFO)
- **Symbol**:	`p`
- **Description**:	A file type for inter-process communication. Data written to the pipe can be read by another process in the order it was written.
- **Examples**:	`/tmp/my_named_pipe`.

## 7. Socket
- **Symbol**:	`s`
- **Description**:	Used for inter-process communication, often over networks. Enables communication between processes, typically over TCP/IP.
- **Examples**:	`/var/run/docker.sock`.

## 8. Door (Solaris specific)
- **Symbol**:	`D`
- **Description**:	A special IPC mechanism found in Solaris, not typically present in Linux.

---

## Additional Information
- **Identifying File Types**:
	- Use `ls -l` to view the file type symbol as the first character in the listing.
	- Use the `file` command to get a more detailed description of the file type.

- **File Type Symbols in `ls -l` Output**:

	| Symbol | File Type         |
	|--------|-------------------|
	| `-`    | Regular file      |
	| `d`    | Directory         |
	| `l`    | Symbolic link     |
	| `c`    | Character device  |
	| `b`    | Block device      |
	| `p`    | Named pipe (FIFO) |
	| `s`    | Socket            |

---

## 3. Working with Files and Directories

## List folders and files in a directory
`ls` - list directory content.

### Useful Options
- `-r`: Reverse order while sorting.
- `-l`: Use long listing format.
- `-t`: Sort by modification time (newest first).
- `-i`: Show inode number.
- `-a`: Include all entries, including hidden files.
- `-d`: List directories themselves, not their contents.
- `-p`: Append a `/` indicator to directories.
- `--group-directories-first`: Group directories before files.

### Additional Information
- `ls --version`, `ls --help`, `info ls`, `man ls`: Access ls command documentation.
- Pipe to `wc -l` to count folders: `ls -d */ | wc -l`.

### Examples
- `ls -l` — List all files and directories with details.
- `ls -a` — Include hidden files.
- `ls -lt` — Sort by modification time.

## `less` — View File Content with Advanced Navigation
- **Purpose**: View file content one screen at a time with more advanced navigation than `more`.
- **Common Options**:
    - `-N`: Show line numbers.
    - `-S`: Do not wrap long lines.
    - `-X`: Disable terminal clearing after exiting.
    - `-f`: Force opening non-regular files (e.g., directories, devices).
- **Navigation**:
    - `Space`: Move forward one screen.
    - `b`: Move backward one screen.
    - `/pattern`: Search forward for `pattern`.
    - `?pattern`: Search backward for `pattern`.
    - `n`: Repeat the last search forward.
    - `N`: Repeat the last search backward.
    - `q`: Quit `less`.
    - `j` or `up arrow`: Move one line up.
    - `k` or `down arrow`: Move one line down.
- **Examples**:
    - `less file.txt` — View `file.txt` with advanced navigation.
    - `cat largefile.txt | less` — Pipe output of `cat` to `less` for paginated viewing.
    - `less -N log.txt` — View `log.txt` with line numbers.

## `more` — View File Content One Screen at a Time
- **Purpose**: Display file content one screen at a time, useful for large files.
- **Common Options**:
    - `-d`: Display a message instead of a beep when an error occurs.
    - `-f`: Count logical lines instead of screen lines (useful for long lines).
    - `-p`: Clear the screen before displaying the next page.
    - `-c`: Do not scroll; instead, repaint the screen from the top.
- **Examples**:
    - `more file.txt` — View `file.txt` one screen at a time.
    - `cat largefile.txt | more` — Pipe output of `cat` to `more` for paginated viewing.

## `tail` — View the End of a File
- **Purpose**: Display the last few lines of a file.
- **Common Options**:
    - `-n [number]`: Specify the number of lines to display (default is 10).
    - `-f`: Follow the file as it grows (useful for logs).
    - `-c [number]`: Display the last specified number of bytes.
- **Examples**:
    - `tail file.txt` — Show the last 10 lines of `file.txt`.
    - `tail -n 20 file.txt` — Show the last 20 lines of `file.txt`.
    - `tail -f log.txt` — Continuously display new lines added to `log.txt`.

## `head` — View the Beginning of a File
- **Purpose**: Display the first few lines of a file.
- **Common Options**:
    - `-n [number]`: Specify the number of lines to display (default is 10).
    - `-c [number]`: Display the first specified number of bytes.
- **Examples**:
    - `head file.txt` — Show the first 10 lines of `file.txt`.
    - `head -n 5 file.txt` — Show the first 5 lines of `file.txt`.
    - `head -c 100 file.txt` — Show the first 100 bytes of `file.txt`.

## Locate file
### 1. `find` Command
- Scans the filesystem in real-time. Highly configurable.
- **Common Options**:
    - `-name pattern`: Search by name.
    - `-type [f|d]`: Search for files (`f`) or directories (`d`).
    - `-size`: Search by file size.
    - `-mtime`: Search by modification time.
- **Example**: `find /home/user -name "*.txt"` — Finds all `.txt` files in `/home/user`.

### 2. `locate` Command
- Uses a pre-built database for faster searches (may be outdated).
- **Common Options**:
    - `-i`: Case-insensitive search.
    - `-c`: Count the number of matches.
    - `-r`: Use regular expressions.
- **Example**: `locate myfile.conf` — Quickly finds paths with `myfile.conf`.

### 3. Updating the Database
- Run `updatedb` (with sudo if needed): `sudo updatedb`

## `pwd` — Print Working Directory
- Displays the full path of the current directory.
- **Example**: `pwd` → `/home/user/documents`

## `cd` — Change Directory
- Navigates between directories.
- **Examples**:
    - `cd /etc` — Change to the `/etc` directory.
    - `cd ..` — Move up one directory level.
    - `cd` or `cd ~` — Go to your home directory.

## File or directory properties
### Example Output of ls -l
>`-rw-r--r--  1  user  group  4096  May 29 12:34  filename.txt`

### Column Breakdown
1. File Permissions (`-rw-r--r--`): Indicates the file type and access permissions.
    - First character: type (`-` = file, `d` = directory, `l` = symlink).
2. Number of Links (`1`): Number of hard links to the file or directory.
3. Owner (`user`): Username of the file’s owner.
4. Group (`group`): Group name that owns the file.
5. File Size (`4096`): Size of the file in bytes.
6. Modification Date and Time (`May 29 12:34`): Last modification date and time.
7. File Name (`filename.txt`): The actual name of the file or directory.

## Create, copy, move, rename and delete files and directories
### 1. `touch` — Create Empty Files or Update Timestamps
- **Purpose**: Create an empty file or update the access/modification time of an existing file.
- **Common Options**:
    - `-c`: Do not create any files; only update timestamps if the file exists.
    - `-t [[CC]YY]MMDDhhmm[.ss]`: Set specific timestamp.
- **Example**: `touch notes.txt` — Creates `notes.txt` if it does not exist.

### 2. `cp` — Copy Files and Directories
- **Purpose**: Copy files or directories.
- **Common Options**:
    - `-r`: Recursively copy directories.
    - `-i`: Prompt before overwrite.
    - `-u`: Copy only when the source file is newer.
    - `-v`: Verbose output showing files as they are copied.
- **Examples**:
    - `cp file1.txt file2.txt` — Copy `file1.txt` to `file2.txt`.
    - `cp -r docs backup/` — Copy the `docs` directory into `backup/`.

### 3. `mkdir` — Make Directories
- **Purpose**: Create new directories.
- **Common Options**:
    - `-p`: Create parent directories as needed, without errors.
- **Example**: `mkdir new_folder` — Creates a folder named `new_folder`.

### 4. `mv` — Move or Rename Files and Directories
- **Purpose**: Move or rename files and directories.
- **Common Options**:
    - `-i`: Prompt before overwrite.
    - `-u`: Move only when source is newer than destination or destination is missing.
    - `-v`: Show each move step in the output.
- **Examples**:
    - `mv file.txt /tmp/` — Move `file.txt` to `/tmp/`.
    - `mv oldname.txt newname.txt` — Rename `oldname.txt` to `newname.txt`.

### 5. `rm` — Remove Files or Directories
- **Purpose**: Delete files and directories.
- **Common Options**:
    - `-r`: Remove directories and their contents recursively.
    - `-f`: Ignore nonexistent files and never prompt.
    - `-i`: Prompt before every removal.
    - `-v`: Explain what is being done (verbose).
- **Examples**:
    - `rm file.txt` — Remove `file.txt`.
    - `rm -r old_folder` — Recursively remove `old_folder` and its contents.

### 6. `rmdir` — Remove Empty Directories
- **Purpose**: Delete empty directories.
- **Common Options**:
    - `--ignore-fail-on-non-empty`: Ignore errors for non-empty directories.
    - `-p`: Remove parent directories if they become empty after removing the target directory.
- **Examples**:
    - `rmdir empty_folder` — Removes the directory `empty_folder` if it is empty.
    - `rmdir -p nested/empty/folder` — Removes `nested/empty/folder` and its parent directories if they are empty.

## `file` — Determine File Type
- **Purpose**: Identify the type of a file (e.g., text, binary, directory).
- **Common Options**:
    - `-b`: Brief output, omitting the filename.
    - `-i`: Output MIME type string.
    - `-z`: Try to look inside compressed files.
- **Examples**:
    - `file image.png` — Output: `image.png: PNG image data`
    - `file script.sh` — Output: `script.sh: Bourne-Again shell script, ASCII text executable`

---

## 4. Permissions, Ownership, and ACLs

# File / Directorys Permissions

## Permission Types
There are **three types of permissions**:
- `r` — **Read**: Permission to view the contents.
- `w` — **Write**: Permission to modify or delete.
- `x` — **Execute**: Permission to run the file (for scripts/binaries) or access a directory.

## Permission Levels
Permissions are assigned for three classes of users:
- `u` — **User** (the file owner)
- `g` — **Group** (users in the same group)
- `o` — **Other** (everyone else)

## Changing Permissions
### Symbolic Method
Use the `chmod` command:
```bash
chmod a-rwx file1 # remove from all the permission to read, write and execute
chmod u+x file1 # add to the current user the permission to execute
chmod g-rx file1 # remove from the current group the permission to read and execute
chmod o-r file1 # remove from other users and groups the permission to read
chmod ugo+r # add to current user, group and others permission to read
```
This sets:
- `a` (all)
- `u` (user)
- `g` (group)
- `o` (others)

### Numeric Method
Permissions are represented numerically:
- `r` = 4, `w` = 2, `x` = 1

To combine permissions, add their values:
- 7 = `rwx` (4+2+1)
- 6 = `rw-` (4+2)
- 5 = `r-x` (4+1)

Example:
```bash
chmod 755 file1
```
Means:
- User: `rwx` (7)
- Group: `rx` (5)
- Others: `rx` (5)

To remove permission user 0:
```bash
chmod 000 file1 # remove all permission from user, group and others
```

## Common chmod Options
- `-R`: Recursively apply to directories and contents
- `-f`: Suppress error messages

# File / Directorys Ownership
Each file has an owner and a group. Ownership can be changed using `chown` and `chgrp`.

## `chown` — Change File Owner
Only the **super-user (root)** can change file ownership.
```bash
sudo chown newuser file.txt
```
Change owner and group:
```bash
sudo chown newuser:newgroup file.txt
```
## Common Options
- `-R`: Recursive
- `-f`: Force (ignore errors)

## `chgrp` — Change Group
Any user can change a file's group to another group they belong to.
```bash
chgrp newgroup file.txt
```
## Common Options
- `-R`: Recursive
- `-f`: Force (ignore errors)

## Tips
- When granting access to a file in a directory, the directory must have execute (`x`) permission for others to traverse it.
- Use `stat filename` for more detailed permission and ownership information.

# Access Control Lists (ACL) in Linux
Access Control Lists (ACLs) in Linux provide a more flexible permission mechanism than traditional UNIX file permissions. ACLs allow fine-grained control by enabling permissions to be set for individual users and groups beyond the standard owner-group-others model.

## What is ACL?
An **Access Control List (ACL)** is a list attached to a file or directory that defines access rights for specific users or groups. This goes beyond the basic permission model (user, group, others) by allowing **multiple users or groups** to have distinct permissions on the same file or directory.

### Why Use ACL?
- Suppose a user is **not a member** of a file’s group, but you still want to give them specific access (e.g., read or write).
- Instead of changing group ownership or using `chmod`, ACLs allow **targeted permissions**.

## Viewing ACLs
To display ACLs on a file:
```bash
getfacl filename
```

### Example
```bash
getfacl test/seinfeld.txt
```

Output:
```bash
# file: test/seinfeld.txt
# owner: iafzal
# group: iafzal
user::rw-
user:iafzal:r-x
group::rw-
other::r--
```

The `user:iafzal:r-x` line indicates an additional user-specific permission entry added via ACL.

> Files with ACLs will display a `+` at the end of their permission string in `ls -l`, e.g.:
> ```bash
> -rw-rwxr--+
> ```

## Setting ACLs with `setfacl`
Use the `setfacl` command to modify ACLs.

### Common Syntax
```bash
setfacl -m u:username:permissions file
setfacl -m g:groupname:permissions file
```

### Key Options
- `-m`: Modify or add ACL entry.
- `-x`: Remove a specific ACL entry.
- `-b`: Remove all ACL entries (reset to standard permissions).
- `-d`: Set default ACLs (for directories; inherited by new files inside).

### Examples
#### Give a user full access to a file
```bash
setfacl -m u:john:rwx mydoc.txt
```

#### Give a group read access
```bash
setfacl -m g:editors:r-- notes.txt
```

#### Set default ACLs on a directory (inherited by new files)
```bash
setfacl -d -m u:john:rwX /shared/folder
```

> The `X` permission is special: it gives execute permission only if the target is a directory or already has execute permission for some user.

## Removing ACLs
### Remove a specific ACL entry
```bash
setfacl -x u:john mydoc.txt
```

### Remove all ACLs
```bash
setfacl -b mydoc.txt
```
After running this command, only the standard UNIX permissions remain.

## Checking ACLs
### With `ls`
Look for a `+` in the permissions string:
```bash
ls -l file.txt
```

Output:
```bash
-rw-r--r--+ 1 user group 1234 Jun 1 14:00 file.txt
```
The `+` indicates extended ACLs are set. Use `getfacl` to see them.

## ACL Tips
- **Default ACLs** on directories make it easier to manage permissions for newly created files.
- ACLs are especially helpful in **collaborative environments**, shared folders, and managed multi-user systems.
- Not all file systems support ACLs by default (e.g., ext4 supports it, but might need to be enabled with the `acl` mount option).

## Summary Table
| Command                      | Description                                   |
|------------------------------|-----------------------------------------------|
| `getfacl file`               | View ACLs of a file                           |
| `setfacl -m u:user:rw file`  | Add or modify ACL for a user                  |
| `setfacl -m g:group:rx file` | Add or modify ACL for a group                 |
| `setfacl -x u:user file`     | Remove a specific ACL entry                   |
| `setfacl -b file`            | Remove all ACL entries                        |
| `setfacl -d -m ...`          | Set default ACLs (inheritance in directories) |

---

## 5. Hard and Soft Links

# Understanding Hard and Soft Links in Linux
In Linux, links are references to files. There are two primary types:

- **Hard Links**: Direct references to the file's data (inode).
- **Soft Links (Symbolic Links or Symlinks)**: References to the file's pathname.

Understanding the differences between these link types is crucial for effective file management.

---

## Hard Links
### Characteristics
- **Inode Sharing**: Hard links share the same inode as the original file, pointing directly to the file's data.
- **Data Persistence**: Deleting the original file does not remove the data as long as at least one hard link exists.
- **File System Constraint**: Hard links cannot span across different file systems.
- **Directory Limitation**: Typically, hard links cannot be created for directories to prevent circular references.
- **Equal Status**: All hard links are equal; there's no concept of an "original" file.

### Common Commands
- **Create a Hard Link**:
	```bash
	ln original_file hard_link
	```
- **List Inode Numbers**:
	```bash
	ls -i original_file hard_link
	```

### Example
```bash
echo "Sample Text" > file1.txt
ln file1.txt file2.txt
rm file1.txt
cat file2.txt  # Outputs: Sample Text
```

---

## Soft Links (Symbolic Links)
### Characteristics
- **Pathname Reference**: Soft links point to the pathname of the target file.
- **Cross-File System**: Can link files across different file systems.
- **Directory Linking**: Can link to directories.
- **Dangling Links**: If the target file is deleted or moved, the soft link becomes broken.
- **Distinct Inode**: Soft links have their own inode and file permissions.

### Common Commands
- **Create a Soft Link**:
	```bash
	ln -s target_file symlink
	Display Link Information:
	```
- **Display Link Information**:
	```bash
	ls -l symlink
	```

### Example
```bash
echo "Sample Text" > file1.txt
ln -s file1.txt link1.txt
rm file1.txt
cat link1.txt  # Error: No such file or directory
```
After deleting `file1.txt`, `link1.txt` becomes a dangling link.

---

## Comparison Table
| Feature                     | Hard Link                    | Soft Link                   |
|-----------------------------|------------------------------|-----------------------------|
| Inode                       | Shares with original         | Has its own                 |
| Reference Type              | Direct to file data          | Points to file name         |
| Cross File System           | No                           | Yes                         |
| Links to Directories        | No (except for `.` and `..`) | Yes                         |
| Affected by Target Deletion | No                           | Yes (becomes dangling)      |
| Creation Command            | `ln original_file hard_link` | `ln -s target_file symlink` |

---

## Use Cases
- Hard Links:
	- Efficient disk space usage when multiple references to the same data are needed.
	- Backup systems to avoid data duplication.

- Soft Links:
	- Creating shortcuts to files or directories.
	- Linking files across different file systems.

---

## Additional Information
- Identifying Links:
	- Use ls -l to identify links. Soft links are indicated by an l at the beginning of the permission string and show the path they point to.
	- Use ls -i to compare inode numbers; hard-linked files share the same inode.

- Removing Links:
	- Hard Link: Deleting a hard link removes that reference. The data persists until all hard links are deleted.
	- Soft Link: Deleting a soft link removes the link without affecting the target file.

---

## 6. Comparing Files

# Compare Files
## `diff` — Compare Files Line-by-Line
`diff` reports differences between two files or directories.

### Useful Options
- `-u`: Produce unified diff format (context lines above/below changes).
- `-c`: Produce context diff (old-style).
- `-r`: Recursively compare sub-directories.
- `-q`: Report only whether files differ (quiet mode).
- `--side-by-side`: Show differences in two-column format.
- `--color`: Enable colored output (GNU diff).

### Additional Information
- Exit status: `0` (no differences), `1` (differences found), `>1` (error).
- Patching: Output from `diff -u` can be fed to `patch` to update files.
- Ignore options: `-w` (ignore whitespace) or `-i` (ignore case) tailor comparisons.
- When comparing directories, `diff -r` shows differing files and passes files to a sub-diff.

### Examples
- `diff fileA.txt fileB.txt` — Standard comparison.
- `diff -u old.c new.c > changes.patch` — Save a unified diff to apply later with `patch`.
- `diff -r dir1 dir2` — Recursively compare two directory trees.

## `cmp` — Byte-by-Byte File Comparison
`cmp` compares two files byte-for-byte and is fastest for verifying exact binary equality.

### Useful Options
- `-l`: List differing bytes (byte position and values).
- `-s`: Silent mode; exit status only.
- `-n NUM`: Compare only the first `NUM` bytes.
- `-i SKIP`: Skip `SKIP` bytes before comparison.

### Additional Information
- Exit status: `0` (identical), `1` (different), `>1` (error).
- Unlike `diff`, `cmp` is ideal for binary files (images, executables).
- The first differing byte is reported in line/byte offsets by default.

### Examples
- `cmp image1.png image2.png` — Report first byte (and line) where they differ.
- `cmp -s iso1.iso iso2.iso && echo "Identical"` — Silent check, echo on success.
- `cmp -n 512 file1.bin file2.bin` — Compare only the first 512 bytes.

---

## 7. Archiving and Compression

# Compress and Decompress
## `tar` — Archive Files and Directories  
`tar` (Tape ARchive) groups multiple files/directories into a single archive (tarball) and can optionally compress it with `gzip` (`-z`) or `bzip2` (`-j`).

### Useful Options  
- `-c` : Create a new archive  
- `-x` : Extract from an archive  
- `-t` : List archive contents  
- `-z` : Compress/decompress with **gzip**  
- `-v` : Verbose output  
- `-f FILE` : Specify archive file name  
- `-C DIR` : Change to directory DIR before operation  

### Additional Information  
- A **.tar.gz** (or **.tgz**) file is simply a tar archive piped through `gzip`.  
- Use `--exclude=PATTERN` to omit files while creating an archive.  

### Examples  
```bash
tar -czvf backup.tgz /home/user         # Create compressed backup
tar -xzvf backup.tgz -C /tmp            # Extract to /tmp
```

## `gzip` — Compress Files  
`gzip` reduces file size using the DEFLATE algorithm, replacing the original file with a `.gz` version.

### Useful Options  
- `-k` : Keep original file  
- `-d` / `--decompress` : Decompress (same as `gunzip`)  
- `-r` : Recursively compress directories  
- `-9 … -1` : Set compression level (9 = best)  

### Additional Information  
- `gzip` keeps file ownership, mode, and timestamp by default  
- Use `-c` to write compressed data to stdout for piping  

### Examples  
```bash
gzip -k access.log                      # Compress log, keep original
tar -cf - dir | gzip > dir.tar.gz       # Stream‑compress and archive
```

## `gunzip` — Decompress .gz Files  
`gunzip` restores compressed files to their original form.

### Useful Options  
- `-c` : Write decompressed data to stdout (keeps `.gz`)  
- `-k` : Keep the `.gz` file after decompression  
- `-l` : List compression statistics  

### Additional Information  
- Multiple files can be decompressed in one call: `gunzip file1.gz file2.gz`  
- To inspect a compressed text file without fully decompressing, combine `gunzip -c` with a pager  

### Examples  
```bash
gunzip -k archive.gz                    # Decompress but keep .gz
gunzip -c notes.txt.gz | less           # View compressed text
```

---

## 8. Input/Output Redirection and Text Processing

## `>` — Redirect Standard Output
- **Purpose**: Write (`overwrite`) the standard output of a command to a file.
- **Common Usage**:
	- `command > file` — Replace `file` with the command’s output.
- **Examples**:
	- `ls > listing.txt` — Saves directory listing to `listing.txt`.
- **Additional Information**:
	- If `file` exists, its previous contents are lost.

## `>>` — Append Redirection
- **Purpose**: Append the standard output of a command to the end of a file without truncating it.
- **Common Usage**:
	- `command >> file` — Add new output to `file`.
- **Examples**:
	- `echo "Backup done" >> /var/log/backup.log` — Adds a time-stamped note to the log.
- **Additional Information**:
	- Useful for maintaining cumulative logs.

## `stdin`, `stdout`, `stderr`
- **Purpose**: Represent the three default I/O streams created for every process:
	- **stdin** (0): standard input.
	- **stdout** (1): standard output.
	- **stderr** (2): standard error.
- **Examples**:
	- `command 2> errors.log` — Redirect only error messages to `errors.log`.
	- `command > out.log 2>&1` — Redirect both stdout and stderr to `out.log`.
- **Additional Information**:
	- Streams can be swapped or combined using redirection operators like `2>&1`.

## `|` — Pipe Operator
- **Purpose**: Send the **stdout** of one command to the **stdin** of another, enabling command chaining.
- **Examples**:
	- `ps aux | grep nginx` — Filters running processes for “nginx”.
	- `cat access.log | sort | uniq -c` — Counts unique lines in `access.log`.

## `tee` — Write to File **and** Standard Output
- **Purpose**: Read from stdin, write its content to both stdout and one or more files simultaneously.
- **Common Options**:
	- `-a` — Append to files instead of overwriting.
- **Examples**:
	- `make 2>&1 | tee build.log` — View build output live while saving it.

## `cat` — Concatenate & Display Files
- **Purpose**: Read, combine, and write file contents to stdout; often used with redirection or pipes.
- **Examples**:
	- `cat file1 file2 > merged.txt` — Creates `merged.txt` containing both files.
	- `cat > notes.txt` — Creates/overwrites `notes.txt`, reading text from stdin until `Ctrl-D`.

## `echo` — Display a Line of Text
- **Purpose**: Print strings or variables to stdout, frequently used for quick output or to generate files via redirection.
- **Common Options**:
	- `-e` — Enable interpretation of backslash escapes.
	- `-n` — Omit the trailing newline.
- **Examples**:
	- `echo "PATH=$PATH" >> ~/.profile` — Append to profile configuration.

## `xargs` — Build & Execute from Standard Input
- **Purpose**: Convert piped input into arguments for another command, overcoming the argument-length limits of simple pipes.
- **Example**:
	- `find . -name "*.log" -print0 | xargs -0 rm` — Delete all `.log` files found.

## Here Documents (`<<`) and Here Strings (`<<<`)
- **Purpose**: Feed multi-line or single-line strings directly into commands as stdin.
- **Examples**:
	- *Here Doc*:
		```bash
		cat <<EOF > config.txt
		key=value
		EOF
		```
	- *Here String*: `grep "error" <<< "$LOG"`

## Quick Reference Table
| Operator / Command | Purpose                              | Example                   |
|--------------------|--------------------------------------|---------------------------|
| `>`                | Redirect stdout (overwrite)          | `echo hi > file`          |
| `>>`               | Redirect stdout (append)             | `echo hi >> file`         |
| `2>`               | Redirect stderr                      | `cmd 2> err.log`          |
| `&>`               | Redirect stdout **and** stderr       | `cmd &> all.log`          |
| `\|`               | Pipe stdout to stdin of next command | `sort data \| uniq -c`    |
| `tee`              | Split stdout to file **and** screen  | `cmd \| tee out.log`      |
| `cat`              | Concatenate & display files          | `cat file1 file2`         |
| `echo`             | Output text to stdout                | `echo $HOME`              |
| `xargs`            | Build command lines from stdin       | `ls *.txt \| xargs wc -l` |
| `<<`, `<<<`        | Provide inline input to commands     | `grep foo <<EOF ... EOF`  |

### Additional Tips
- Combine operators (`>` `2>&1`) to capture both output streams together.
- To **append** while also seeing live output, use `tee -a filename`.
- Many commands support a built-in help page via the `--help` flag for quick option lookup.

---

## 9. Running Multiple Commands

# Running Multiple Commands in Linux
Linux shells provide several operators and built‑in commands for chaining, grouping, and controlling multiple commands in one line or script.

## `;` — Command Separator
- **Purpose**: Execute commands **sequentially**; the next command runs regardless of the previous command’s exit status.
- **Example**:  
  ```bash
  echo "update" ; apt-get update
  ```

## `&&` — Logical AND
- **Purpose**: Run the second command **only if** the first exits with status 0 (success).
- **Example**:  
  ```bash
  make && make install
  ```

## `||` — Logical OR
- **Purpose**: Run the next command **only if** the previous command **fails** (non‑zero exit status).
- **Example**:  
  ```bash
  grep pattern file.txt || echo "Pattern not found"
  ```

## `&` — Background Operator
- **Purpose**: Execute a command in the **background**, returning the prompt immediately so other commands can be entered.
- **Example**:  
  ```bash
  long_task &  # Runs while you continue working
  ```

## `wait` — Wait for Background Jobs
- **Purpose**: Pause script execution until specified background job(s) finish. Without arguments, waits for **all** child jobs.
- **Example**:  
  ```bash
  cmd1 & pid=$!
  cmd2
  wait $pid
  ```

## `|` — Pipe
- **Purpose**: Send the **stdout** of one command to the **stdin** of another, enabling command chaining and transformation.
- **Example**:  
  ```bash
  cat access.log | sort | uniq -c
  ```

## `tee` — Split Output
- **Purpose**: Read stdin and write to **both** stdout and file(s), useful for logging while piping.
- **Example**:  
  ```bash
  make 2>&1 | tee build.log
  ```

## `()` — Subshell Grouping
- **Purpose**: Run a list of commands in a **subshell**; variable changes do not persist in the parent shell.
- **Example**:  
  ```bash
  (cd /tmp && tar xzf archive.tgz)
  ```

## `{ }` — Current‑Shell Grouping
- **Purpose**: Execute grouped commands in the **current** shell (no subshell). Must end with a semicolon or newline before `}`.
- **Example**:  
  ```bash
  { echo "Start"; date; } > log.txt
  ```

## `xargs` — Build Command Lines from Stdin
- **Purpose**: Convert piped input into arguments for another command, enabling batch operations.
- **Example**:  
  ```bash
  find . -name "*.log" -print0 | xargs -0 rm
  ```

## Running Multiline Commands
- **Purpose**: Write commands across multiple lines for better readability and organization. Useful in scripts or complex one-liners.
- **Syntax**: Use a backslash (`\`) at the end of each line to indicate continuation.
- **Example**:  
	```bash
	echo "Starting process" && \
	mkdir -p /tmp/mydir && \
	cd /tmp/mydir && \
	touch file.txt
	```
- **Note**: Ensure there are no trailing spaces after the backslash.

## Error Handling Tip
Combine `&&` and `||` for basic control flow:  
```bash
command1 && echo "Success" || echo "Failure"
```

## Quick Reference Table
| Operator / Command | Purpose                             | Runs in Subshell? | Continue on Error? |
|--------------------|-------------------------------------|-------------------|--------------------|
| `;`                | Sequential execution                | No                | Yes                |
| `&&`               | Run next **if previous succeeds**   | No                | No                 |
| `\|\|`             | Run next **if previous fails**      | No                | No (opposite)      |
| `&`                | Run command in background           | No                | —                  |
| `()`               | Group commands in **subshell**      | Yes               | N/A                |
| `{ }`              | Group commands in current shell     | No                | N/A                |
| `\|`               | Pipe stdout → stdin                 | Depends           | Propagates status  |
| `tee`              | Split stdout to file **and** screen | No                | N/A                |
| `xargs`            | Build command lines from stdin      | No                | N/A                |
| `wait`             | Wait for background jobs            | No                | N/A                |

---

## 10. Getting Help

## `man` — Display Command Manual
- **Purpose**: Provides detailed documentation for Linux commands, including their usage, options, and examples.
- **Common Options**:
    - `-k keyword`: Search for commands related to a keyword.
    - `-f command`: Display a brief description of a command.
    - `-P pager`: Specify a pager program (e.g., `less` or `more`) to view the manual.
    - `-a`: Show all matching manual pages in sequence.

- **Examples**:
    - `man ls` — Displays the manual for the `ls` command.
    - `man -k copy` — Searches for commands related to "copy".
    - `man -f bash` — Displays a brief description of the `bash` command.

- **Additional Information**:
    - Manual pages are organized into sections. For example:
        - Section 1: User commands.
        - Section 5: File formats and conventions.
        - Section 8: System administration commands.
    - To view a specific section, use `man [section] command`. For example, `man 5 passwd` shows the manual for the `passwd` file format.
    - Use `/` to search within the manual page and `q` to quit.

---

## `whatis` — Brief Command Descriptions
- **Purpose**: Displays a one-line description of a command, similar to `man -f`.
- **Common Usage**:
    - `whatis command`

- **Examples**:
    - `whatis ls` — Returns a short summary like “ls (1) - list directory contents”.
    - `whatis mkdir` — Shows the description of the `mkdir` command.

- **Additional Information**:
    - Uses the `whatis` database, which may need to be updated using the `makewhatis` or `mandb` command.
    - Useful for quickly identifying what a command does without opening a full manual page.

---

## `--help` — Command-Line Help Flag
- **Purpose**: Provides a summary of a command’s syntax, options, and usage directly in the terminal.
- **Common Usage**:
    - `command --help`

- **Examples**:
    - `ls --help` — Lists all available options for the `ls` command.
    - `cp --help` — Displays usage instructions for the `cp` command.

- **Additional Information**:
    - Almost all GNU/Linux commands support `--help`.
    - The output is typically shorter and more concise than `man`, making it useful for quick reference.
    - Ideal for scripting and automation, where a full manual might be unnecessary.


---

# Summary of Wildcards in Linux
## 1. What are Wildcards?
- **Purpose**:	Wildcards are special characters that allow pattern matching in file and directory names, enabling efficient file management and command execution.
- **Usage Context**:	Utilized in shell commands like `ls`, `cp`, `mv`, `rm`, and others to operate on multiple files matching specific patterns.

## 2. Common Wildcards
### a. Asterisk (`*`)
- **Function**:	Matches zero or more characters.
- **Examples**:
	- `*.txt`:	Matches all files ending with `.txt`.
	- `data*`:	Matches files starting with `data`.
	- `*log*`:	Matches files containing `log` anywhere in the name.

### b. Question Mark (`?`)
- **Function**:	Matches exactly one character.
- **Examples**:
	- `file?.txt`:	Matches `file1.txt`, `fileA.txt`, but not `file10.txt`.
	- `??.sh`:	Matches files with two-character names ending with `.sh`.

### c. Square Brackets (`[ ]`)
- **Function**:	Matches any one character within the brackets.
- **Examples**:
	- `file[1-3].txt`:	Matches `file1.txt`, `file2.txt`, `file3.txt`.
	- `file[a-c].txt`:	Matches `filea.txt`, `fileb.txt`, `filec.txt`.
	- `file[!a-c].txt`:	Matches files not ending with `a`, `b`, or `c`.

### d. Curly Braces (`{ }`)
- **Function**:	Performs brace expansion, generating a set of strings.
- **Examples**:
	- `file{1,2,3}.txt`:	Expands to `file1.txt`, `file2.txt`, `file3.txt`.
	- `{*.jpg,*.png}`:	Matches all `.jpg` and `.png` files.

### e. Tilde (`~`)
- **Function**:	Represents the current user's home directory.
- **Example**:
	- `cd ~/documents`:	Changes directory to `documents` in the home directory.

## 3. Advanced Wildcard Usage
### a. Recursive Matching with `**`
- **Function**:	Matches directories and files recursively.
- **Example**:
	- `ls **/*.txt`:	Lists all `.txt` files in the current directory and all subdirectories.
- **Note**:	Requires shell support for globstar (e.g., `shopt -s globstar` in Bash).

### b. Combining Wildcards
- **Function**:	Allows complex pattern matching by combining different wildcards.
- **Example**:
	- `file[0-9][a-z]?.*`:	Matches files starting with a digit, followed by a lowercase letter, then any single character, and any extension.

## 4. Practical Applications
- **Listing Files**:
	- `ls *.txt`:	Lists all `.txt` files.
	- `ls data?.csv`:	Lists files like `data1.csv`, `dataA.csv`.

- **Copying Files**:
	- `cp *.jpg images/`:	Copies all `.jpg` files to the `images` directory.
	- `cp report[1-3].doc backups/`:	Copies `report1.doc`, `report2.doc`, and `report3.doc` to `backups`.

- **Moving Files**:
	- `mv *.log logs/`:	Moves all `.log` files to the `logs` directory.

- **Deleting Files**:
	- `rm temp*`:	Deletes all files starting with `temp`.
	- `rm *~`:	Deletes all backup files ending with `~`.

## 5. Important Considerations
- **Shell Expansion**:	Wildcards are expanded by the shell before the command is executed. The command receives the list of matching files as arguments.
- **Quoting**:	To prevent wildcard expansion, enclose patterns in quotes. For example, `'*.txt'` will be treated as a literal string.
- **Escaping Special Characters**:	Use backslash (`\`) to escape special characters. For example, `\*` represents a literal asterisk.

---

## Text Processors

## `cut` — Select Columns or Fields
`cut` extracts sections from each line of input.

### Useful Options
- `-b LIST`: Select byte positions. 
- `-c LIST`: Select characters. 
- `-d DELIM`: Specify a field delimiter (default is TAB). 
- `-f LIST`: Select fields (requires `-d`). 

### Additional Information
- Use `--complement` to output everything **except** the selected fields. 

### Examples
- `cut -d':' -f1 /etc/passwd` — Show user names.  
- `cut -c1-3 file.txt` — Display the first three characters of each line.

## `awk` — Pattern-Scanning and Processing
`awk` is a powerful text-processing language that scans input line by line, splits it into fields, and performs actions. 

### Useful Options
- `-F FS`: Set input field separator. 
- `-v var=value`: Pass variable to the program. 
- `-f script.awk`: Read program from file. 

### Additional Information
- Built-in variables like `NR` (record number) and `NF` (field count) assist in processing.

### Examples
- `awk -F',' '{print $1,$3}' data.csv` — Print columns 1 and 3.  
- `awk '$3>80' scores.txt` — Show lines where the third field > 80.

## `grep` / `egrep` — Search Text with Regex
`grep` searches files for lines matching a pattern. `egrep` is shorthand for `grep -E` (extended regex).

### Useful Options
- `-i`: Case-insensitive search.
- `-n`: Show line numbers.
- `-v`: Invert match (show non-matching lines).
- `-E`: Use extended regex (same as `egrep`).
- `-r`: Recursive search.

### Additional Information
- `grep -o` prints only the matching part of a line. 
### Examples
- `grep -i error *.log` — Find “error” in all logs.
- `egrep 'cat|dog' pets.txt` — Match “cat” **or** “dog”.

## `sort` — Order Lines of Text
`sort` organizes lines from stdin or files. 

### Useful Options
- `-n`: Numeric sort.
- `-r`: Reverse order. 
- `-k FIELD`: Sort by specific field. 
- `-u`: Output unique lines (equivalent to `sort | uniq`). 

### Additional Information
- Combine with `LC_ALL=C` for faster byte-wise sorting. 

### Examples
- `sort -nr -k5 file.txt` — Numeric reverse sort on field 5.  
- `sort names.txt > sorted.txt` — Save alphabetized list.

## `uniq` — Filter Adjacent Duplicates
`uniq` reports or removes repeated lines that are **adjacent**. Usually combined with `sort`. 

### Useful Options
- `-c`: Prefix duplicates with count. 
- `-d`: Show only repeated lines. 
- `-u`: Show only unique lines. 

### Additional Information
- Use `sort | uniq` to handle non-adjacent duplicates. 

### Examples
- `sort file | uniq -c` — Count unique lines.  
- `uniq -u list.txt` — Display lines that occur once.

## `wc` — Word, Line, and Byte Count
`wc` counts lines, words, characters, bytes, and maximum line length. 

### Useful Options
- `-l`: Lines.  
- `-w`: Words.  
- `-c`: Bytes.  
- `-m`: Characters (can differ from bytes in UTF-8). 

### Additional Information
- Combine with pipelines for on-the-fly statistics. 

### Examples
- `wc -l *.py` — Count lines in all Python files.  
- `grep -o 'error' *.log | wc -l` — Count occurrences of “error”.