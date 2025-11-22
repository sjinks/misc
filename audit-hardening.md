## Ensure audit configuration files mode is configured

Access to the audit configuration files could allow unauthorized personnel to prevent the auditing of critical events.
Misconfigured audit configuration files may prevent the auditing of critical events or degrade system performance by overwhelming the audit log.
Misconfiguration of the audit configuration files may also make it more challenging to establish and investigate events relating to an incident.

```sh
chown root:root /etc/audit/rules.d/*.rules
chmod 0640 /etc/audit/rules.d/*.rules
```

mitre_mitigations: M1022  
mitre_tactics: TA0007  
mitre_techniques: T1070, T1070.002, T1083  
