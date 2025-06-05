## `date` — Display or Set System Date and Time
- **Purpose**: Show or set the system’s date and time. Can format output, parse strings, and adjust for different time zones.
- **Common Options**:
	- `+FORMAT`: Format output according to the given `FORMAT` string (e.g., `date "+%Y-%m-%d %H:%M:%S"`).
	- `-d, --date=STRING`: Display the date/time described by `STRING` instead of the current; supports natural-language expressions (e.g., `date -d "next Friday"`).
	- `-u, --utc`: Display or set time in Coordinated Universal Time (UTC) rather than local time.
	- `-s, --set=STRING`: Set the system clock to the date/time described by `STRING` (requires root privileges).
	- `--rfc-3339=FMT`: Output date/time in RFC 3339 format (e.g., `--rfc-3339=seconds`).
	- `-R, --rfc-email`: Output date and time in RFC 5322 format suitable for email headers.
- **Additional Information**:
	- By default, the output respects the system locale (e.g., `Thu Jun 5 14:23:00 UTC 2025` or localized format).
	- Common format specifiers include:
	- `%Y` – Year (four digits)
	- `%m` – Month (two digits)
	- `%d` – Day of month (two digits)
	- `%H` – Hour (00–23)
	- `%M` – Minute (00–59)
	- `%S` – Second (00–60)
	- `%A` – Full weekday name
	- `%B` – Full month name
	- Frequently used in scripts to generate timestamps or log file names (e.g., `$(date +%F-%H%M)`).
	- Modifying system time manually can impact running processes; network time synchronization (NTP) is recommended for most production systems.
- **Examples**:
	- `date` Display the current date and time.
	- `date "+%Y-%m-%d %H:%M:%S"` Print `2025-06-05 14:23:00`.
	- `date -d "2025-12-25"` Show the date corresponding to Christmas 2025.
	- `sudo date -s "2025-06-01 09:00:00"` Set the system clock to June 1, 2025 at 09:00.

---

## `uptime` — Show How Long the System Has Been Running
- **Purpose**: Display the current time, system uptime (how long it has been running), number of logged-in users, and load averages for the past 1, 5, and 15 minutes.
- **Common Options**:
	- `-p, --pretty`: Show uptime in a human-readable format (e.g., “up 3 hours, 12 minutes”).
	- `-s, --since`: Display the date and time when the system was last started (boot time).
	- `-h, --help`: Show usage help and exit.
	- `-V, --version`: Show version information and exit.
- **Additional Information**:
	- Load averages represent the average number of processes waiting for CPU over 1, 5, and 15 minutes. Values above the number of CPU cores suggest high load.
	- `uptime` reads from `/proc/uptime` to determine the elapsed seconds since boot.
	- Often used in monitoring scripts or by administrators to gauge system load and health at a glance.
- **Examples**:
	- `uptime` Output example: `14:30:01 up 5 days, 3:42, 2 users, load average: 0.15, 0.10, 0.05`.
	- `uptime -p` Output example: `up 5 days, 3 hours, 42 minutes`.
	- `uptime -s` Output example: `2025-05-31 10:42:00`.

---

## `hostname` — Show or Set the System’s Hostname
- **Purpose**: Display the current hostname or set a new one. The hostname is a human-readable identifier for the machine on a network.
- **Common Options**:
	- `hostname`  
	  Print the current hostname.
	- `-s, --short`: Show only the short hostname (the part before the first dot).
	- `-f, --fqdn`, `--long`: Display the fully qualified domain name (FQDN).
	- `-d, --domain`: Display the DNS domain name.
	- `-i, --ip-address`: Display the IP address(es) associated with the hostname.
	- `-I, --all-ip-addresses`: Display all network addresses of the host.
	- `-b, --boot`: Always set a hostname (bypass cache).
	- `-F, --file FILE`: Read or write the hostname to/from `FILE` (commonly `/etc/hostname`).
	- `-h, --help`: Show help text and exit.
	- `-v, --version`: Show version information and exit.
- **Additional Information**:
	- To persistently change the hostname, edit `/etc/hostname` (Debian/Ubuntu) or `/etc/sysconfig/network` (RHEL/CentOS), then reboot or use `hostnamectl set-hostname`.
	- On `systemd`-based systems, `hostnamectl` is preferred for managing static, pretty, and transient hostnames.
	- Valid hostnames consist of letters, digits, and hyphens and must not contain spaces or special characters.
- **Examples**:
	- `hostname` Display: `myserver`.
	- `hostname -f` Display: `myserver.example.com`.
	- `sudo hostname newhostname` Temporarily change the hostname to `newhostname` until reboot or `/etc/hostname` is updated.
	- `hostname -i` Display the IP address associated with `myserver`.

---

## `uname` — Display System Information
- **Purpose**: Print information about the current system, including kernel name, network node hostname, kernel release, kernel version, machine hardware name, processor type, hardware platform, and operating system.
- **Common Options**:
	- `-a, --all`: Print all available system information.
	- `-s, --kernel-name`: Print the kernel name (e.g., `Linux`).
	- `-n, --nodename`: Print the network node hostname.
	- `-r, --kernel-release`: Print the kernel release (e.g., `5.4.0-142-generic`).
	- `-v, --kernel-version`: Print the kernel version (build details).
	- `-m, --machine`: Print the machine hardware name (e.g., `x86_64`).
	- `-p, --processor`: Print the processor type (if available).
	- `-i, --hardware-platform`: Print the hardware platform (if available).
	- `-o, --operating-system`: Print the operating system name.
- **Additional Information**:
	- Useful for scripting conditionals based on architecture or kernel version (e.g., `if [ "$(uname -m)" = "x86_64" ]; then ... fi`).
	- On many Linux distributions, `uname -a` shows a line such as:
	  ```bash
	  Linux myserver 5.4.0-142-generic #156-Ubuntu SMP Tue May 13 11:05:29 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
	  ```
- **Examples**:
	- `uname -a` Display all system information.
	- `uname -r` Show the kernel release.
	- `uname -m`  
	  Show the machine hardware name.
	- `uname -s` Show the kernel name (e.g., `Linux`).

---

## `which` — Locate Executable in `PATH`
- **Purpose**: Search the directories listed in the user’s `PATH` environment variable and return the full path of the executable that would be run if the command name is invoked.
- **Common Options**:
	- `which <command>` Print the path to the first matching executable in `PATH`.
	- `-a, --all`: Print all matching executables found in `PATH`, not only the first.
	- `-s, --skip-dot`: Skip entries in `PATH` that begin with `.` (e.g., `./script`).
- **Additional Information**:
	- Helps verify which version of a utility is used when multiple versions exist.
	- Some shells have a built-in `which`; the external `/usr/bin/which` may behave slightly differently.
	- If nothing is printed, the command is not in `PATH` or `which` is aliased to another tool (such as `type`).
- **Examples**:
	- `which ls` Output: `/usr/bin/ls`.
	- `which -a python` Output might include multiple paths:  
	  ```bash
	  /usr/bin/python
	  /usr/local/bin/python
	  ```
	- `which --skip-dot myscript.sh`  
	  Ensure `myscript.sh` is found only in system paths, not `.` directories.

---

## `cal` — Display a Calendar
- **Purpose**: Show a formatted calendar in the terminal for a specified month and year, or for the entire year.
- **Common Options**:
	- `cal` Display the current month’s calendar.
	- `cal [month] [year]` Display the calendar for a specific month and year (e.g., `cal 12 2025`).
	- `-3` Show the previous, current, and next month side by side.
	- `-y [year]` Show the entire year’s calendar (e.g., `cal -y 2025`).
	- `-m [month]` Show a specific month of the current year (e.g., `cal -m 7` for July).
- **Additional Information**:
	- By default, weeks start on Sunday; the `ncal` command uses Monday as the first day.
	- Some implementations highlight the current day or week.
	- Useful for quick date reference in scripts or when scheduling tasks.
- **Examples**:
	- `cal` Display the current month’s calendar.
	- `cal 06 2025` Show June 2025.
	- `cal -3` Display May, June, and July 2025 side by side.
	- `cal -y 2025` Display the entire calendar for 2025.

---

## `bc` — Arbitrary Precision Calculator Language
- **Purpose**: Provide an interactive calculator with support for arbitrary precision arithmetic, suitable for both interactive use and scripting.
- **Common Options**:
	- `-l, --mathlib` Load the standard math library (provides functions like `sqrt`, `sine`, `cosine`, etc.).
	- `-q, --quiet` Suppress the normal welcome message when starting `bc`.
	- `-w, --warn` Enable warnings for extensions beyond the POSIX `bc` standard.
	- `-i, --interactive` Force interactive mode, even if input is piped.
	- `-h, --help` Display help text and exit.
	- `-v, --version` Print version information and exit.
- **Additional Information**:
	- `bc` supports variables, conditional statements (`if`), loops (`while`), and user-defined functions.
	- The built-in variable `scale` sets the number of decimal places for division and other operations (e.g., `scale=5; 22/7`).
	- Useful in shell scripts for performing floating-point calculations since most shells only handle integer arithmetic by default.
- **Examples**:
	- `bc` Enter interactive calculator mode.
	- `echo "5 * (7 + 3)" | bc` Output: `50`.
	- `echo "scale=4; 22/7" | bc -l` Output: `3.1428`.
	- `bc -l` Start `bc` with the math library loaded.