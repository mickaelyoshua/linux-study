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