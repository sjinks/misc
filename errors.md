## Ensure core dumps are restricted

A core dump is the memory of an executable program.
It is generally used to determine why a program aborted.
It can also be used to glean confidential information from a core file.
The system allows setting a soft limit for core dumps, but the user can override it.
Setting a hard limit on core dumps prevents users from overriding the soft variable.
If core dumps are required, consider setting limits for user groups (see [limits.conf(5)](https://linux.die.net/man/5/limits.conf)).
In addition, setting the `fs.suid_dumpable` variable to `0` will prevent setuid programs from dumping core.

```sh
printf '%s\n' 'fs.suid_dumpable = 0' >> /etc/sysctl.d/99-hardening.conf
sysctl -w fs.suid_dumpable=0

printf '%s\n' '* hard core 0' >> /etc/security/limits.d/hardening.conf
```

If `systemd-coredump` is installed and enabled: set the following in `/etc/systemd/coredump.conf`:
```
Storage=none
ProcessSizeMax=0 
```

and run `systemctl daemon-reload`

mitre_mitigations: [Data Loss Prevention (M1057)](https://attack.mitre.org/mitigations/M1057/)  
mitre_tactics: [Discovery (TA0007)](https://attack.mitre.org/tactics/TA0007/)  
mitre_techniques: [Data from Local System (T1005)](https://attack.mitre.org/techniques/T1005/)  

## Ensure Automatic Error Reporting is not enabled

The Apport Error Reporting Service automatically generates crash reports for debugging.
Apport collects potentially sensitive data, such as core dumps, stack traces, and log files.
They can contain passwords, credit card numbers, serial numbers, and other private material.

```sh
apt-get purge apport
```
