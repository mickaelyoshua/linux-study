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