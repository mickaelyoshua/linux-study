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