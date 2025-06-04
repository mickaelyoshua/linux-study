# File / Directorys Permissions

## Permission Types
There are **three types of permissions**:
- `r` — **Read**: Permission to view the contents.
- `w` — **Write**: Permission to modify or delete.
- `x` — **Execute**: Permission to run the file (for scripts/binaries) or access a directory.

## Permission Levels
Permissions are assigned for three classes of users:
- `u` — **User** (the file owner)
- `g` — **Group** (users in the same group)
- `o` — **Other** (everyone else)

## Changing Permissions
### Symbolic Method
Use the `chmod` command:
```bash
chmod a-rwx file1 # remove from all the permission to read, write and execute
chmod u+x file1 # add to the current user the permission to execute
chmod g-rx file1 # remove from the current group the permission to read and execute
chmod o-r file1 # remove from other users and groups the permission to read
chmod ugo+r # add to current user, group and others permission to read
```
This sets:
- `a` (all)
- `u` (user)
- `g` (group)
- `o` (others)

### Numeric Method
Permissions are represented numerically:
- `r` = 4, `w` = 2, `x` = 1

To combine permissions, add their values:
- 7 = `rwx` (4+2+1)
- 6 = `rw-` (4+2)
- 5 = `r-x` (4+1)

Example:
```bash
chmod 755 file1
```
Means:
- User: `rwx` (7)
- Group: `rx` (5)
- Others: `rx` (5)

To remove permission user 0:
```bash
chmod 000 file1 # remove all permission from user, group and others
```

## Common chmod Options
- `-R`: Recursively apply to directories and contents
- `-f`: Suppress error messages

# File / Directorys Ownership
Each file has an owner and a group. Ownership can be changed using `chown` and `chgrp`.

## `chown` — Change File Owner
Only the **super-user (root)** can change file ownership.
```bash
sudo chown newuser file.txt
```
Change owner and group:
```bash
sudo chown newuser:newgroup file.txt
```
## Common Options
- `-R`: Recursive
- `-f`: Force (ignore errors)

## `chgrp` — Change Group
Any user can change a file's group to another group they belong to.
```bash
chgrp newgroup file.txt
```
## Common Options
- `-R`: Recursive
- `-f`: Force (ignore errors)

## Tips
- When granting access to a file in a directory, the directory must have execute (`x`) permission for others to traverse it.
- Use `stat filename` for more detailed permission and ownership information.

# Access Control Lists (ACL) in Linux
Access Control Lists (ACLs) in Linux provide a more flexible permission mechanism than traditional UNIX file permissions. ACLs allow fine-grained control by enabling permissions to be set for individual users and groups beyond the standard owner-group-others model.

## What is ACL?
An **Access Control List (ACL)** is a list attached to a file or directory that defines access rights for specific users or groups. This goes beyond the basic permission model (user, group, others) by allowing **multiple users or groups** to have distinct permissions on the same file or directory.

### Why Use ACL?
- Suppose a user is **not a member** of a file’s group, but you still want to give them specific access (e.g., read or write).
- Instead of changing group ownership or using `chmod`, ACLs allow **targeted permissions**.

## Viewing ACLs
To display ACLs on a file:
```bash
getfacl filename
```

### Example
```bash
getfacl test/seinfeld.txt
```

Output:
```bash
# file: test/seinfeld.txt
# owner: iafzal
# group: iafzal
user::rw-
user:iafzal:r-x
group::rw-
other::r--
```

The `user:iafzal:r-x` line indicates an additional user-specific permission entry added via ACL.

> Files with ACLs will display a `+` at the end of their permission string in `ls -l`, e.g.:
> ```bash
> -rw-rwxr--+
> ```

## Setting ACLs with `setfacl`
Use the `setfacl` command to modify ACLs.

### Common Syntax
```bash
setfacl -m u:username:permissions file
setfacl -m g:groupname:permissions file
```

### Key Options
- `-m`: Modify or add ACL entry.
- `-x`: Remove a specific ACL entry.
- `-b`: Remove all ACL entries (reset to standard permissions).
- `-d`: Set default ACLs (for directories; inherited by new files inside).

### Examples
#### Give a user full access to a file
```bash
setfacl -m u:john:rwx mydoc.txt
```

#### Give a group read access
```bash
setfacl -m g:editors:r-- notes.txt
```

#### Set default ACLs on a directory (inherited by new files)
```bash
setfacl -d -m u:john:rwX /shared/folder
```

> The `X` permission is special: it gives execute permission only if the target is a directory or already has execute permission for some user.

## Removing ACLs
### Remove a specific ACL entry
```bash
setfacl -x u:john mydoc.txt
```

### Remove all ACLs
```bash
setfacl -b mydoc.txt
```
After running this command, only the standard UNIX permissions remain.

## Checking ACLs
### With `ls`
Look for a `+` in the permissions string:
```bash
ls -l file.txt
```

Output:
```bash
-rw-r--r--+ 1 user group 1234 Jun 1 14:00 file.txt
```
The `+` indicates extended ACLs are set. Use `getfacl` to see them.

## ACL Tips
- **Default ACLs** on directories make it easier to manage permissions for newly created files.
- ACLs are especially helpful in **collaborative environments**, shared folders, and managed multi-user systems.
- Not all file systems support ACLs by default (e.g., ext4 supports it, but might need to be enabled with the `acl` mount option).

## Summary Table
| Command                      | Description                                   |
|------------------------------|-----------------------------------------------|
| `getfacl file`               | View ACLs of a file                           |
| `setfacl -m u:user:rw file`  | Add or modify ACL for a user                  |
| `setfacl -m g:group:rx file` | Add or modify ACL for a group                 |
| `setfacl -x u:user file`     | Remove a specific ACL entry                   |
| `setfacl -b file`            | Remove all ACL entries                        |
| `setfacl -d -m ...`          | Set default ACLs (inheritance in directories) |