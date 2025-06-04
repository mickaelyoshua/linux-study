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
| `||`               | Run next **if previous fails**      | No                | No (opposite)      |
| `&`                | Run command in background           | No                | —                  |
| `()`               | Group commands in **subshell**      | Yes               | N/A                |
| `{ }`              | Group commands in current shell     | No                | N/A                |
| `|`                | Pipe stdout → stdin                 | Depends           | Propagates status  |
| `tee`              | Split stdout to file **and** screen | No                | N/A                |
| `xargs`            | Build command lines from stdin      | No                | N/A                |
| `wait`             | Wait for background jobs            | No                | N/A                |