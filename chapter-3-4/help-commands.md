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