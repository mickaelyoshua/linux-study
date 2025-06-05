# User Account Management

## `useradd` — Create a New User Account
- **Purpose**: The `useradd` command creates a new user account on a Linux system, initializing the home directory, shell, and default group memberships.
- **Common Options**:
	- `-d HOME_DIR`: Specify a custom home directory rather than the default (`/home/username`).
	- `-m`: Create the user’s home directory if it does not already exist.
	- `-s SHELL`: Set the user’s default login shell (e.g., `/bin/bash`).
	- `-c COMMENT`: Add a comment (e.g., full name) to the user’s GECOS field.
	- `-G GROUP1,GROUP2,…`: Add the new user to supplementary group(s).
	- `-e YYYY-MM-DD`: Set an expiration date for the user account.
	- `-u UID`: Specify a numeric user ID (UID) instead of automatically assigning the next available UID.
	- `-b BASE_DIR`: Prepend a base directory to the default home directory if `-d` is not specified.
	- `--badname`: Allow usernames that do not conform to naming standards.
- **Additional Information**:
	- Configuration defaults (home directory skeleton, default shell) are defined in `/etc/default/useradd` and `/etc/login.defs`.
	- On Debian-based systems, the higher-level `adduser` script is often used, which in turn calls `useradd`.
	- Creating a user requires root privileges (e.g., using `sudo`).
- **Examples**:
	- `sudo useradd -m -s /bin/bash alice` Create user “alice” with a home directory and Bash shell.
	- `sudo useradd -d /srv/data/bob -m -c "Bob Smith" bob` Create user “bob” with home set to `/srv/data/bob` and comment “Bob Smith.”
	- `sudo useradd -G sudo,developers -m carol` Create user “carol,” adding her to the “sudo” and “developers” groups.
	- `sudo useradd -u 1500 -e 2025-12-31 -m dave` Create user “dave” with UID 1500 and expiration date 2025-12-31.

---

## `groupadd` — Create a New Group
- **Purpose**: The `groupadd` command creates a new group on a Linux system, assigning a unique group ID (GID) and optionally marking it as a system group.
- **Common Options**:
	- `-g GID`: Specify a numeric group ID instead of using the next available GID.
	- `-r`: Create a system group (GID below the range defined in `/etc/login.defs`).
	- `-f, --force`: If the group already exists, exit successfully; if used with `-g` and the specified GID is taken, choose a new GID automatically.
	- `-K KEY=VALUE`: Override default values from `/etc/login.defs` (e.g., `GID_MIN`, `GID_MAX`).
- **Additional Information**:
	- Group definitions are stored in `/etc/group` (format: `group_name:x:GID:user_list`).
	- Creating a group requires root privileges.
	- Avoid leading hyphens or special characters in group names to prevent parsing issues.
- **Examples**:
	- `sudo groupadd developers` Create a new group named “developers” with the next available GID.
	- `sudo groupadd -g 1005 designers` Create group “designers” with GID 1005.
	- `sudo groupadd -r sysgroup` Create a system group named “sysgroup.”
	- `sudo groupadd -K GID_MIN=2000 -K GID_MAX=3000 newgroup` Create “newgroup” with a GID selected from the range 2000-3000.

---

## `usermod` — Modify an Existing User Account
- **Purpose**: The `usermod` command changes attributes of an existing user account (such as home directory, login name, group memberships, shell, and expiration date).
- **Common Options**:
	- `-c COMMENT`: Update the comment field (GECOS) in `/etc/passwd`.
	- `-d HOME_DIR`: Change the user’s home directory; with `-m`, move current contents to the new directory.
	- `-s SHELL`: Set a new login shell for the user.
	- `-l NEW_LOGIN`: Change the user’s login name (username).
	- `-u UID`: Change the user’s numeric UID.
	- `-g GROUP`: Change the user’s primary group.
	- `-G GROUP1,GROUP2,…`: Set supplementary groups (overwriting existing). Use `-a` to append instead of overwrite.
	- `-a, --append`: When used with `-G`, append the user to the specified supplementary groups rather than replacing them.
	- `-e YYYY-MM-DD`: Set or change the account expiration date.
	- `-L`: Lock the user’s password (prepend “!” in `/etc/shadow`), preventing login.
	- `-U`: Unlock the user’s password (remove “!”).
	- `-p PASSWORD`: Set an encrypted password in `/etc/shadow` (passing encrypted form).
	- `--badname`: Allow a username that does not conform to naming rules.
	- `-r, --remove`: When used with `-G`, remove the user from named supplementary groups.
- **Additional Information**:
	- `usermod` updates several files: `/etc/passwd`, `/etc/shadow`, `/etc/group`, and `/etc/gshadow`.
	- Only root (or via `sudo`) can run `usermod`. Changing UID or username can break file ownership; it may be necessary to manually update file ownership with `find` and `chown`.
- **Examples**:
	- `sudo usermod -c "John Doe" john` Add comment “John Doe” to user “john.”
	- `sudo usermod -d /home/johndoe -m john` Change “john”’s home directory to `/home/johndoe` and move contents.
	- `sudo usermod -l jdoe jane` Rename user “jane” to “jdoe.”
	- `sudo usermod -a -G sudo,developers alice` Add “alice” to supplementary groups “sudo” and “developers.”
	- `sudo usermod -L testuser` Lock the “testuser” account.
	- `sudo usermod -U testuser` Unlock the “testuser” account.

---

## `userdel` — Remove a User Account
- **Purpose**: The `userdel` command deletes an existing user account, optionally removing the user’s home directory and mail spool.
- **Common Options**:
	- `-r`: Remove the user’s home directory and mail spool.
	- `-f, --force`: Force removal of the user account even if the user is currently logged in; also removes home directory and mail spool.
	- `-h, --help`: Display help message and exit.
	- `-R, --root CHROOT_DIR`: Apply changes in a specified `CHROOT_DIR`; use configuration files from that directory.
	- `-P, --prefix PREFIX_DIR`: Apply changes in `PREFIX_DIR` without `chroot`; useful for cross-compile targets.
	- `-Z`: Remove any SELinux user mapping for the account.
- **Additional Information**:
	- Deleting a user does not remove files they own outside their home directory; these must be located and reassigned or removed manually.
	- After deletion, crontab entries remain; remove them with `crontab -r -u USERNAME`.
	- Exit status: `0` on success, `1` if a standard error occurred, or `>1` if other errors.
- **Examples**:
	- `sudo userdel bob` Delete user “bob” but retain bob’s home directory and mail spool.
	- `sudo userdel -r bob` Delete “bob” and remove `/home/bob` plus mail spool.
	- `sudo userdel -f alice` Force deleting “alice” even if she is logged in.
	- `sudo userdel -R /mnt/chroot testuser` Delete “testuser” in the `/mnt/chroot` environment using that directory’s configuration.

---

## `groupdel` — Remove a Group
- **Purpose**: The `groupdel` command deletes an existing group from the system, removing its entry from `/etc/group` and `/etc/gshadow`.
- **Common Options**:
	- `-f, --force`: Force removal of the group even if any user has it as their primary group.
	- `-h, --help`: Display help message and exit.
	- `-R, --root CHROOT_DIR`: Apply changes in a specified `CHROOT_DIR`.
	- `-P, --prefix PREFIX_DIR`: Apply changes in `PREFIX_DIR` without `chroot`.
	- `--extrausers`: Make `groupdel` use the extra users database (non-standard).
- **Additional Information**:
	- A group cannot be deleted if it is the primary group of any user, unless `-f` is used.
	- After deletion, files owned by the deleted GID will retain numeric group ownership until reassigned.
- **Examples**:
	- `sudo groupdel developers` Delete group “developers.”
	- `sudo groupdel -f sysadmin` Force delete “sysadmin” even if it is a primary group for a user.
	- `sudo groupdel -R /mnt/chroot oldgroup` Delete “oldgroup” in the `/mnt/chroot` environment.

---

## `passwd` — Change User Password
- **Purpose**: The `passwd` command updates a user’s authentication token (password) and password aging information.
- **Common Options**:
	- `-l`: Lock the user’s password (prepend “!” in `/etc/shadow`), preventing login.
	- `-u`: Unlock the user’s password, reversing `-l`.
	- `-d`: Delete the user’s password, allowing passwordless login if other authentication methods permit.
	- `-e`: Expire the user’s password immediately, forcing a password change on next login.
	- `-n MIN_DAYS`: Set minimum number of days between password changes.
	- `-x MAX_DAYS`: Set maximum number of days a password remains valid.
	- `-w WARN_DAYS`: Set number of days of warning before password expiry.
	- `-i INACTIVE_DAYS`: Set number of days after password expiry until account lock.
	- `-S`: Display password status information (e.g., last changed, expire date).
- **Additional Information**:
	- Running `passwd` without options changes the invoking user’s password interactively.
	- Administrators (`sudo`) can change other users’ passwords by specifying the username (e.g., `sudo passwd bob`).
	- Password policy settings (default expiry, warning) are defined in `/etc/login.defs` and can be overridden per user.
- **Examples**:
	- `passwd` Change your own password (prompts for current and new).
	- `sudo passwd alice` Administratively set a new password for user “alice.”
	- `sudo passwd -l bob` Lock user “bob”’s password immediately.
	- `sudo passwd -u bob` Unlock user “bob”’s password.
	- `sudo passwd -e carol` Expire “carol”’s password to force a change on next login.

---

## `chage` — Change User Password Aging Information
- **Purpose**: Edit password aging settings for a user account, including last password change date, expiration date, warning days, and inactivity period.
- **Common Options**:
	- `-l, --list` Display the current password aging information for the specified user.
	- `-d, --lastday LAST_DAY` Set the date (YYYY-MM-DD or days since Jan 1, 1970) when the password was last changed. A value of `0` forces a password change at next login.
	- `-E, --expiredate EXPIRE_DATE` Specify the account expiration date (YYYY-MM-DD or days since Jan 1, 1970). Passing `-1` removes the expiration date.
	- `-m, --mindays MIN_DAYS` Set the minimum number of days between password changes; a value of `0` allows immediate changes.
	- `-M, --maxdays MAX_DAYS` Define the maximum number of days a password remains valid; when exceeded, the user must change the password.
	- `-W, --warndays WARN_DAYS` Set how many days in advance of expiration a warning is shown to the user.
	- `-I, --inactive INACTIVE` Set the number of days after password expiration until the account is permanently disabled; a value of `-1` disables inactivity lock.
	- `-i, --iso8601` Output dates in ISO 8601 format (YYYY-MM-DD).
	- `-h, --help` Display a help message summarizing `chage` options and exit.
- **Additional Information**:
	- Password aging data is stored in `/etc/shadow`.  
	- System-wide defaults for password expiration (e.g., `PASS_MAX_DAYS`, `PASS_MIN_DAYS`, `PASS_WARN_AGE`) are found in `/etc/login.defs`.  
	- Only the superuser (root) or a user with `sudo` privileges can modify another user’s aging information.  
	- Invoking `chage` without options or with `-l` shows fields like last change, expiry date, inactivity, account expiry, minimum and maximum days and warning days.  
- **Examples**:
	- `sudo chage -l jdoe` List all password aging settings for user “jdoe.”
	- `sudo chage -d 2023-01-01 alice` Force user “alice” to change password on next login by setting her last change date to Jan 1, 2023.
	- `sudo chage -M 90 -m 7 -W 10 bob` For user “bob,” set minimum days between changes to 7, maximum days to 90, and warn 10 days before expiration.
	- `sudo chage -E 2025-12-31 carol` Expire “carol”’s account on Dec 31, 2025 (after which login is disabled).
	- `sudo chage -I 30 dave` Lock “dave”’s account 30 days after his password has expired.
	- `sudo chage --warndays 14 -M 60 emma` Require “emma” to change her password every 60 days and show warnings starting 14 days before expiration.

---

## User Files
- `/etc/passwd` = This file has all user’s attributes
- `/etc/shadow` = This file contains encrypted user password and password policy
- `/etc/group` = All group and user group information

## `su` — Switch User
- **Purpose**: The `su` (substitute user) command allows you to start a shell as another user (by default root), executing commands under that user’s identity.
- **Common Options**:
	- `su [username]` Start a shell as the specified user. If no username is provided, defaults to root.
	- `su - [username]` Start a login shell for the target user, loading their environment (`~/.profile`, `~/.bash_profile`, etc.).
	- `-c COMMAND` Execute a single `COMMAND` under the target user’s account and then exit.
	- `-s SHELL` Run the specified shell instead of the target user’s default shell.
	- `-m` or `-p` Preserve the invoking user’s environment variables (HOME, SHELL, etc.) instead of switching to the target user’s environment.
- **Additional Information**:
	- After running `su`, you are prompted for the target user’s password (or root’s if no user is specified).
	- Using `su -` ensures the target user’s environment (PATH, locale, shell startup files) is fully initialized.
	- To return to your original user, type `exit` or press `Ctrl+D`.
	- On many distributions, only users in a privileged group (e.g., “wheel” or “sudo”) can switch directly to root via `su`.
- **Examples**:
	- `su` Switch to root and start an interactive root shell.
	- `su - alice` Switch to user “alice” with her full login environment.
	- `su -c "apt-get update && apt-get upgrade" root` Run the update commands as root, then return to the invoking user.

---

## `sudo` — Execute Command as Another User
- **Purpose**: The `sudo` command (superuser do) allows an authorized user to execute a single command as root (or another user) based on the rules defined in `/etc/sudoers`.
- **Common Options**:
	- `sudo command`  
	  Run `command` as root, prompting for the invoking user’s own password.
	- `-u USER`, `--user=USER`  
	  Execute the command as the specified `USER` instead of root.
	- `-i`, `--login` Run a login shell as the target user, loading that user’s environment.
	- `-s`, `--shell` Start a shell as the target user without loading a full login environment.
	- `-l`, `--list` List the commands the current user is allowed (and not allowed) to run via `sudo`.
	- `-v`, `--validate` Update the user’s cached credentials without running a command (prompts for password if needed).
	- `-k`, `--reset-timestamp` Invalidate the user’s cached credentials immediately; next `sudo` will prompt for a password again.
- **Additional Information**:
	- Users must be defined in the `sudoers` file or in a file within `/etc/sudoers.d/` to use `sudo`.
	- By default, `sudo` authenticates by asking for the invoking user’s own password, not the target user’s.
	- Successful `sudo` usage is logged (typically in `/var/log/auth.log` or `/var/log/secure`).
	- Environment variables like `SUDO_USER` (original username) and `SUDO_UID` are preserved to track the invoking user.
	- Password prompts can be cached for a short timeout (usually 5–15 minutes) to allow multiple `sudo` invocations without re-entering the password.
- **Examples**:
	- `sudo apt-get install nginx` Install the Nginx package using root privileges.
	- `sudo -u www-data systemctl restart apache2` Restart the Apache service as the user “www-data.”
	- `sudo -l` Show the list of commands the current user is permitted to run.
	- `sudo -k` Invalidate cached credentials; next `sudo` will require a password.
	- `sudo -i` Open an interactive root shell (similar to `su -` but using the invoking user’s password).

---

## `visudo` — Safely Edit the `/etc/sudoers` File
- **Purpose**: The `visudo` command securely edits the `/etc/sudoers` file (or an alternate “sudoers” file) by locking it and performing a syntax check on save to prevent misconfiguration.
- **Common Usage**:
	- `sudo visudo`  
	  Open the main `/etc/sudoers` file in the default editor (selected from `$EDITOR`, `$VISUAL`, or `$SUDO_EDITOR`).
	- `sudo visudo -f /etc/sudoers.d/custom`  
	  Edit a specific sudoers fragment within `/etc/sudoers.d/`.
- **Additional Information**:
	- `visudo` places an exclusive lock on the sudoers file to prevent simultaneous edits.
	- When you save changes, `visudo` runs a syntax check; if errors are found, it warns you and does not overwrite the existing file.
	- Common default editors are `vi`, `vim`, or `nano`, depending on the distribution and environment variables.
	- Instead of modifying `/etc/sudoers` directly, best practice is to place small configuration files in `/etc/sudoers.d/` (each ending in `.conf`) so they can be managed separately.
	- Sudoers entries follow the format:
	  ```bash
	  user	host = (run-as-user) commands
	  ```
	  For example:
	  ```bash
	  alice   ALL = (ALL) ALL
	  %wheel  ALL = (ALL) ALL
	  ```
- **Examples**:
	- `sudo visudo`  
	  Safely edit the main sudoers file; any syntax errors are caught before saving.
	- `sudo visudo -f /etc/sudoers.d/developer`  
	  Edit (or create) a sudoers fragment for the “developer” group.

---

## `/etc/sudoers` and `/etc/sudoers.d/`
- **Purpose**: These locations define which users or groups are allowed to run commands via `sudo`, under what conditions, and whether they need a password.
- **Details**:
	- The primary file `/etc/sudoers` holds global sudo policies; it must be edited only through `visudo`.
	- The directory `/etc/sudoers.d/` allows modular files—typically one per service or user—that are included by the main sudoers file. This simplifies updates and reduces merge conflicts.
	- Files in `/etc/sudoers.d/` should be owned by root, have permissions `0440`, and usually end in `.conf` (distribution-dependent).
	- A basic sudoers entry might look like:
	  ```bash
	  bob	 ALL = (ALL) NOPASSWD: /usr/bin/systemctl restart nginx
	  ```
	  This allows “bob” to restart Nginx on any host without requiring a password.
- **Security Considerations**:
	- Never edit `/etc/sudoers` or files in `/etc/sudoers.d/` with a regular text editor; always use `visudo` to catch syntax errors.
	- Use the least-privilege principle—grant only the minimum commands needed with `sudo`, and avoid `NOPASSWD` unless absolutely necessary.
	- Periodically audit all sudoers entries to ensure no unauthorized or overly broad privileges exist.

## `who` — Show Who Is Logged In
- **Purpose**: Display a list of users currently logged into the system.
- **Common Options**:
	- `-H`: Show column headers.
	- `-q`: Only display usernames and the number of users.
	- `-b`: Show the last system boot time.
	- `-u`: Show idle time for each user.
- **Additional Information**:
	- Reads from the `/var/run/utmp` file to determine active sessions.
	- Useful for quickly seeing which accounts have active shells or graphical sessions.
- **Examples**:
	- `who` Lists all currently logged-in users and their terminals.
	- `who -H` Includes headers above each column (USER, TTY, DATE, TIME, COMMENT).
	- `who -q` Prints just usernames and a count, e.g., `alice bob carol 3 users`.
	- `who -b` Displays the last boot time, e.g., `system boot  2025-06-05 08:23`.

---

## `last` — Show Last Logged-In Users
- **Purpose**: Display a history of user logins, reboots, and shutdowns from the `/var/log/wtmp` file.
- **Common Options**:
	- `-n COUNT`, `-COUNT`: Show the most recent `COUNT` entries.
	- `-a`: Display hostname on the last column.
	- `-d`: Display full login and logout dates (rather than relative).
	- `-x`: Include system shutdown entries and runlevel changes.
	- `-F`: Show full login and logout times with year.
- **Additional Information**:
	- By default, lists entries in reverse chronological order until the file's end.
	- Can be combined with a username to filter, e.g., `last alice`.
	- Useful for auditing login history or troubleshooting.
- **Examples**:
	- `last` Lists recent logins, reboots, and shutdowns.
	- `last -n 10` Show only the last ten entries.
	- `last -x` Include system shutdown and runlevel change entries.
	- `last -a` Appends remote hostname or IP to each entry (for remote ssh sessions).

---

## `w` — Show Who Is Logged In and What They Are Doing
- **Purpose**: Display a summary of currently logged-in users along with their activity (processes, CPU usage, login time, idle time, etc.).
- **Common Options**:
	- `-h`: Omit the header line (useful in scripts).
	- `-s`: Short output, without column headings or blank lines.
	- `-f`: Do not show the login hostname field.
- **Additional Information**:
	- Combines functionality of `who` with an overview of each user's running processes.
	- The first line shows system uptime, load averages, and number of users.
	- Subsequent lines include: USER, TTY, FROM, LOGIN@, IDLE, JCPU, PCPU, WHAT.
- **Examples**:
	- `w` Display all users and their current activity.
	- `w -h` Show user activity without the header line.
	- `w -s` Condensed output, listing only user, tty, idle, and command.

---

## `id` — Show User Identity
- **Purpose**: Display the user’s effective UID, GID, and group memberships.
- **Common Options**:
	- `-u`: Print only the effective user ID (UID).
	- `-g`: Print only the effective group ID (GID).
	- `-G`: Print all group IDs to which the user belongs.
	- `-n`: Combine with `-u`, `-g`, or `-G` to print names instead of numeric IDs.
- **Additional Information**:
	- By default, shows the current user’s UID, primary GID, and all supplementary group IDs.
	- Can specify a username to query another user, e.g., `id alice`.
	- Handy for verifying group membership for permissions or sudo access.
- **Examples**:
	- `id` Show current user’s UID, GID, and group memberships.
	- `id -u` Display just the numeric UID.
	- `id -nG` List all group names of which the current user is a member.
	- `id bob` Show identity information for user “bob.”

---

## `finger` — User Information Lookup
- **Purpose**: Display detailed information about local or remote users, including login name, real name, home directory, shell, last login, and more.
- **Common Options**:
	- `-l`: Long format; show full details such as mail status, plan file, project file.
	- `-m`: Match user by login name exactly (useful if multiple matches).
	- `-s`: Short format for brief information (login, real name, tty, idle, login time, office, office phone).
- **Additional Information**:
	- Requires the `finger` service or local `/etc/passwd` entries; sometimes disabled on modern distributions due to privacy/security concerns.
	- May include “Plan” and “Project” files stored in the user’s home directory under `.plan` and `.project`.
	- Often used in multi-user, networked environments for retrieving user contact info.
- **Examples**:
	- `finger` List information for all users currently logged in.
	- `finger alice` Show detailed info for user “alice.”
	- `finger -l bob` Display extended info (home directory, shell, last login) for user “bob.”
	- `finger -s` Show a concise summary of currently logged-in users (login, name, tty, idle, etc.).

## Special Permissions: `setuid`, `setgid`, and `Sticky Bit`

Linux supports three “special” file mode bits that modify standard permission behavior. They are applied with `chmod` and are visible in the file’s permission string as `s`, `S`, or `t` characters.

---

### `setuid` (Set User ID on Execution)
- **Purpose**: When the `setuid` bit is set on an executable, any user who runs that program temporarily acquires the file owner’s user ID (usually root) for the duration of the process. This allows non-privileged users to execute certain commands with elevated privileges.
- **How to Set / Remove**:
  - **Set**:  
	```bash
	sudo chmod u+s /path/to/program
	```
	After setting, the owner’s execute bit becomes `s` in the permission string (e.g., `-rwsr-xr-x`).
  - **Remove**:  
	```bash
	sudo chmod u-s /path/to/program
	```
	The permission reverts to a normal execute bit (`x`).
- **Additional Information**:
  - The file owner must have execute permission for `setuid` to take effect; if the owner’s execute bit is not set, the `s` appears as an uppercase `S` and has no effect.
  - Common examples: `/usr/bin/passwd` and `/usr/bin/sudo` use `setuid root` so that users can change their passwords or run commands as root.
  - **Security Considerations**: `setuid root` programs must be carefully audited for vulnerabilities; a buffer overflow or other flaw could allow privilege escalation.
- **Examples**:
  - `sudo chmod 4755 /usr/bin/myapp` Sets `setuid` on `myapp`, making its ownership bits show as `-rwsr-xr-x`.
  - `ls -l /usr/bin/passwd` Typical output: `-rwsr-xr-x 1 root root ... /usr/bin/passwd`.

---

### `setgid` (Set Group ID on Execution or Directory Inheritance)
- **Purpose (Executable)**: When set on an executable, any user running that program temporarily acquires the file’s group ID for the process.  
- **Purpose (Directory)**: When set on a directory, files and subdirectories created within inherit the directory’s group rather than the primary group of the creating user.
- **How to Set / Remove**:
  - **Set**:  
	```bash
	sudo chmod g+s /path/to/file-or-directory
	```
	For executables, the group execute bit becomes `s` (e.g., `-rwxr-sr-x`). For directories, the bit appears as a lowercase `s` in the group position (e.g., `drwxrwsr-x`).
  - **Remove**:  
	```bash
	sudo chmod g-s /path/to/file-or-directory
	```
- **Additional Information**:
  - For executables, if the owner’s group does not have execute permission, `S` shows and has no effect.
  - On directories, `setgid` ensures collaborative environments (e.g., shared project folders) keep files owned by a common group.
  - **Security Considerations**: `setgid` executables can also be used for privilege elevation if the group is privileged. Directory `setgid` is generally lower risk, but be aware of group ownership inheritance.
- **Examples**:
  - `sudo chmod 2755 /usr/bin/someshared` Sets `setgid` on the executable `someshared` (permissions become `-rwxr-sr-x`).
  - `sudo chmod 2770 /srv/project` Make `/srv/project` a `setgid` directory (`drwxrws---`) so that all new files inherit the `project` group.
  - `stat /srv/project` Confirms group ownership and `setgid` bit for newly created files.

---

### Sticky Bit
- **Purpose (Directory)**: When set on a directory, the sticky bit restricts deletion or renaming of files within that directory so that only the file owner, the directory owner, or root can remove or rename them. Commonly used on shared directories like `/tmp`.
- **How to Set / Remove**:
  - **Set**:  
	```bash
	sudo chmod +t /path/to/directory
	```
	The sticky bit appears as a `t` in the “other” execute position (e.g., `drwxrwxrwt`).
  - **Remove**:  
	```bash
	sudo chmod -t /path/to/directory
	```
	The `t` reverts to a normal execute bit (`x`).
- **Additional Information**:
  - If the execute bit for “other” is not set, the `t` appears as an uppercase `T` and has no effect.
  - Common example:  
	```bash
	ls -ld /tmp
	```
	Typically shows: `drwxrwxrwt 10 root root ... /tmp`.
  - **Security Considerations**: Prevents users from deleting other users’ files in shared directories. Essential for world-writable directories.
- **Examples**:
  - `sudo chmod 1777 /shared/public` Creates a world-writable directory (`drwxrwxrwt`) in which only file owners can delete their own files.
  - `stat /shared/public` Verifies the sticky bit (`t`) is set in the permissions.
