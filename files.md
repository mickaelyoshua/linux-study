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

## Locate file
### 1. `find` Command
- Scans the filesystem in real-time.
- Can be slow for large filesystems or deep searches.
- Example: `find / -name data.txt` — searches the entire filesystem for `data.txt`.
- Highly configurable with many options to limit scope and improve efficiency.

### 2. `locate` Command
- Uses a pre-built database (snapshot) for faster searches.
- Example: `locate data.txt` — returns results much faster than `find`.
- Limitation: Data may be outdated (up to 24 hours old), as the database is updated periodically.
- Uses `updatedb` to refresh the database.

### 3. Key Differences
- **Speed**: `locate` is faster but may show outdated results.
- **Freshness**: `find` always shows current data.
- **Database dependency**: `locate` requires `updatedb`; `find` does not.
- **Configurability**: `find` offers extensive options for refining search parameters.

### 4. Updating the Database
- Run `updatedb` to sync the locate database and ensure accurate results.

## `pwd` — Print Working Directory
- Displays the full path of the current directory.
- Usage: `pwd`
- Useful for confirming your current location in the filesystem.

## `cd` — Change Directory
- Navigates between directories.
- Usage:
	- `cd /path/to/directory` — change to specific directory.
	- `cd ..` — move up one level.
	- `cd` or `cd ~` — go to the home directory.
- Useful for moving around the filesystem.