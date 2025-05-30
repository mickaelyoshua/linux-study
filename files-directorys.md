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

## `file` — Determine File Type
- **Purpose**: Identify the type of a file (e.g., text, binary, directory).
- **Common Options**:
    - `-b`: Brief output, omitting the filename.
    - `-i`: Output MIME type string.
    - `-z`: Try to look inside compressed files.
- **Examples**:
    - `file image.png` — Output: `image.png: PNG image data`
    - `file script.sh` — Output: `script.sh: Bourne-Again shell script, ASCII text executable`