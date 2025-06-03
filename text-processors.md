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