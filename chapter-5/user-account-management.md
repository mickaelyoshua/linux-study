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