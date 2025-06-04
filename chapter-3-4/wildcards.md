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