# Cron Hardening

```bash
dpkg-statoverride --update --add root root 0600 /etc/crontab
dpkg-statoverride --update --add root root 0700 /etc/cron.d
dpkg-statoverride --update --add root root 0700 /etc/cron.daily
dpkg-statoverride --update --add root root 0700 /etc/cron.hourly
dpkg-statoverride --update --add root root 0700 /etc/cron.monthly
dpkg-statoverride --update --add root root 0700 /etc/cron.weekly
```

---
mitre_mitigations: [M1018 (User Account Management)](https://attack.mitre.org/mitigations/M1018/)  
mitre_tactics: [TA0002 (Execution)](https://attack.mitre.org/tactics/TA0002/), [TA0007 (Discovery)](https://attack.mitre.org/tactics/TA0007/)  
mitre_techniques: [T1053 (Scheduled Task/Job: Cron)](https://attack.mitre.org/techniques/T1053/), [T1053.003 (Scheduled Task/Job: Cron)](https://attack.mitre.org/techniques/T1053/003/)  
