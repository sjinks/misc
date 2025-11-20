# Audit Rules

In the rules below, `1000` in `auid>=1000` corresponds to the value of `UID_MIN` from `/etc/login.defs`.

## Ensure changes to system administration scope (sudoers) are collected

Changes in the `/etc/sudoers` and `/etc/sudoers.d` files can indicate that an unauthorized change has been made to the scope of system administrator activity.

```
-w /etc/sudoers -p wa -k scope
-w /etc/sudoers.d -p wa -k scope
```

mitre_mitigations: M1047  
mitre_tactics: TA0004  
mitre_techniques: T1562, T1562.006  

## Ensure actions as another user are always logged

Creating an audit log of users with temporarily elevated privileges and the operation(s) they performed is essential to reporting.
Administrators will want to correlate the events written to the audit trail with the records written to sudo's logfile to verify if unauthorized commands have been executed.

```
-a always,exit -F arch=b64 -C euid!=uid -F auid!=unset -S execve -k user_emulation
-a always,exit -F arch=b32 -C euid!=uid -F auid!=unset -S execve -k user_emulation
```

mitre_mitigations: M1047  
mitre_tactics: TA0004  
mitre_techniques: T1562, T1562.006  

## Ensure events that modify the sudo log file are collected

Changes in `/var/log/sudo.log` indicate that an administrator has executed a command or the log file itself has been tampered with.
Administrators will want to correlate the events written to the audit trail with the records written to `/var/log/sudo.log` to verify if unauthorized commands have been executed.

This requires `Defaults logfile="/var/log/sudo.log"` in sudo configuration.

```
-w /var/log/sudo.log -p wa -k sudo_log_file
```

mitre_tactics: TA0004  
mitre_techniques: T1562, T1562.006  

## Ensure events that modify date and time information are collected

Unexpected changes to the system date and/or time may indicate malicious activity on the system.

```
-a always,exit -F arch=b64 -S adjtimex,settimeofday -k time-change
-a always,exit -F arch=b32 -S adjtimex,settimeofday -k time-change
-a always,exit -F arch=b64 -S clock_settime -F a0=0x0 -k time-change
-a always,exit -F arch=b32 -S clock_settime -F a0=0x0 -k time-change
-w /etc/localtime -p wa -k time-change
```

mitre_mitigations: M1047  
mitre_tactics: TA0005  
mitre_techniques: T1562, T1562.006  

## Ensure events that modify the system's network environment are collected

Monitoring system events that change network environments, such as `sethostname` and `setdomainname`, helps identify unauthorized changes to host and domain names, which could compromise security settings that rely on these names.
Changes to `/etc/hosts` can signal unauthorized attempts to alter the machine's IP address associations, potentially redirecting users and processes to unintended destinations.
Monitoring `/etc/issue` and `/etc/issue.net` is crucial for detecting intruders who insert false information to deceive users.
Monitoring `/etc/network/` reveals modifications to network interfaces or scripts that may jeopardize system availability or security.
Additionally, tracking changes in the `/etc/netplan/` directory ensures swift detection of unauthorized adjustments to network configurations.

```
-a always,exit -F arch=b64 -S sethostname,setdomainname -k system-locale
-a always,exit -F arch=b32 -S sethostname,setdomainname -k system-locale
-w /etc/issue -p wa -k system-locale
-w /etc/issue.net -p wa -k system-locale
-w /etc/hosts -p wa -k system-locale
-w /etc/networks -p wa -k system-locale
-w /etc/network/ -p wa -k system-locale
-w /etc/netplan/ -p wa -k system-locale
-a always,exit -F dir=/etc/NetworkManager/ -F perm=wa -F key=system-locale
```

mitre_mitigations: M1047  
mitre_tactics: TA0003  
mitre_techniques: T1562, T1562.006  

## Ensure unsuccessful file access attempts are collected

Failed attempts to open, create, or truncate files may indicate that an individual or process is attempting to gain unauthorized access to the system.

```
-a always,exit -F arch=b64 -S creat,open,openat,openat2,open_by_handle_at,truncate,ftruncate -F exit=-EACCES -F auid>=1000 -F auid!=unset -k access
-a always,exit -F arch=b64 -S creat,open,openat,openat2,open_by_handle_at,truncate,ftruncate -F exit=-EPERM -F auid>=1000 -F auid!=unset -k access
-a always,exit -F arch=b32 -S creat,open,openat,openat2,open_by_handle_at,truncate,ftruncate -F exit=-EACCES -F auid>=1000 -F auid!=unset -k access
-a always,exit -F arch=b32 -S creat,open,openat,openat2,open_by_handle_at,truncate,ftruncate -F exit=-EPERM -F auid>=1000 -F auid!=unset -k access
```

mitre_mitigations: M1047  
mitre_tactics: TA0007  
mitre_techniques: T1562,T1562.006  

## Ensure events that modify user/group information are collected

Unexpected changes to these files may indicate that the system has been compromised and that an unauthorized user is attempting to hide their activities or compromise additional accounts.

```
-w /etc/group -p wa -k identity
-w /etc/passwd -p wa -k identity
-w /etc/gshadow -p wa -k identity
-w /etc/shadow -p wa -k identity
-w /etc/security/opasswd -p wa -k identity
-w /etc/nsswitch.conf -p wa -k identity
-w /etc/pam.conf -p wa -k identity
-w /etc/pam.d -p wa -k identity
```

mitre_mitigations: M1047  
mitre_tactics: TA0004  
mitre_techniques: T1562,T1562.006  

## Ensure discretionary access control permission modification events are collected

Monitoring changes in file attributes could alert a system administrator to activity that may indicate intruder activity or a policy violation.

```
-a always,exit -F arch=b64 -S chmod,fchmod,fchmodat -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b32 -S chmod,fchmod,fchmodat -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S chown,fchown,lchown,fchownat -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b32 -S lchown,fchown,chown,fchownat -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b64 -S setxattr,lsetxattr,fsetxattr,removexattr,lremovexattr,fremovexattr -F auid>=1000 -F auid!=unset -k perm_mod
-a always,exit -F arch=b32 -S setxattr,lsetxattr,fsetxattr,removexattr,lremovexattr,fremovexattr -F auid>=1000 -F auid!=unset -k perm_mod
```

mitre_mitigations: M1022  
mitre_tactics: TA0005  
mitre_techniques: T1562, T1562.006  

## Ensure successful file system mounts are collected

It is highly unusual for a non-privileged user to mount file systems on the system.

```
-a always,exit -F arch=b64 -S mount -F auid>=1000 -F auid!=unset -k mounts
-a always,exit -F arch=b32 -S mount -F auid>=1000 -F auid!=unset -k mounts
```

mitre_mitigations: M1034  
mitre_tactics: TA0010  
mitre_techniques: T1562,T1562.006  

## Ensure session initiation information is collected

Monitoring these files for changes could alert a system administrator to logins occurring at unusual hours, which could indicate intruder activity (i.e., a user logging in at a time when they do not generally log in).
  - `/var/run/utmp` tracks all currently logged in users.
  - `/var/log/wtmp` tracks logins, logouts, shutdowns, and reboots.
  - `/var/log/btmp` keeps track of failed login attempts and can be read by entering the command `/usr/bin/last -f /var/log/btmp`

```
-w /var/run/utmp -p wa -k session
-w /var/log/wtmp -p wa -k session
-w /var/log/btmp -p wa -k session
```

mitre_mitigations: M1047  
mitre_tactics: TA0001  
mitre_techniques: T1562, T1562.006  

## Ensure login and logout events are collected

Monitoring login/logout events could provide a system administrator with information about brute-force attacks on user logins.
  - `/var/log/lastlog` maintains records of the last time a user successfully logged in.
  - `/var/run/faillock` is a directory that maintains records of login failures via the `pam_faillock` module.

```
-w /var/log/lastlog -p wa -k logins
-w /var/run/faillock -p wa -k logins
```

mitre_mitigations: M1047  
mitre_tactics: TA0001  
mitre_techniques: T1562, T1562.006  

## Ensure file deletion events by users are collected

Monitoring these calls from non-privileged users could provide a system administrator with evidence that inappropriate removal of protected files and their associated file attributes is occurring.
While this audit option will examine all events, system administrators should look for specific privileged files that are being deleted or altered.

```
-a always,exit -F arch=b64 -S rename,unlink,unlinkat,renameat -F auid>=1000 -F auid!=unset -k delete
-a always,exit -F arch=b32 -S rename,unlink,unlinkat,renameat -F auid>=1000 -F auid!=unset -k delete
```

mitre_mitigations: M1047  
mitre_tactics: TA0005  
mitre_techniques: T1562, T1562.006  

## Ensure events that modify the system's Mandatory Access Controls are collected

Changes to files in the `/etc/apparmor/` and `/etc/apparmor.d/` directories could indicate that an unauthorized user is attempting to modify access controls or change security contexts, leading to a system compromise.

```
-w /etc/apparmor/ -p wa -k MAC-policy
-w /etc/apparmor.d/ -p wa -k MAC-policy
```

mitre_mitigations: M1022  
mitre_tactics: TA0004  
mitre_techniques: T1562, T1562.006  

## Ensure successful and unsuccessful attempts to use the `chcon`, `setfacl`, `chacl`, and `usermod` commands are collected

The `chcon` command changes a file's security context.
The `setfacl` command sets Access Control Lists (ACLs) of files and directories.
The `chacl` command changes the ACL(s) for a file or directory.
The `usermod` command modifies the system account files to reflect the changes that are specified on the command line.
Without generating audit records tailored to the organization's security and mission needs, it would not be easy to establish, correlate, and investigate events related to an incident or identify those responsible for it.
Audit records can be generated from various components within the information system (e.g., module or policy filter).

```
-a always,exit -F path=/usr/bin/chcon -F perm=x -F auid>=1000 -F auid!=unset -k perm_chng
-a always,exit -F path=/usr/bin/setfacl -F perm=x -F auid>=1000 -F auid!=unset -k perm_chng
-a always,exit -F path=/usr/bin/chacl -F perm=x -F auid>=1000 -F auid!=unset -k perm_chng
-a always,exit -F path=/usr/sbin/usermod -F perm=x -F auid>=1000 -F auid!=unset -k usermod
```

mitre_mitigations: M1022  
mitre_tactics: TA0005  
mitre_techniques: T1562, T1562.006  

## Ensure kernel module loading, unloading, and modification are collected

Monitoring the various ways to manipulate kernel modules could provide system administrators with evidence of an unauthorized change to a kernel module, potentially compromising system security.
All module loading/listing/dependency checking is handled by `kmod` via symbolic links. The following system calls control loading and unloading of modules:
  - `init_module` loads a module;
  - `finit_module` loads a module (used when the overhead of using cryptographically signed modules to determine the authenticity of a module can be avoided)
  - `delete_module` deletes a module;
  - `create_module` creates a loadable module entry;
  - `query_module` queries the kernel for various bits about modules.

```
-a always,exit -F arch=b64 -S init_module,finit_module,delete_module,create_module,query_module -F auid>=1000 -F auid!=unset -k kernel_modules
-a always,exit -F path=/usr/bin/kmod -F perm=x -F auid>=1000 -F auid!=unset -k kernel_modules
```
