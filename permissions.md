## Ensure permissions on bootloader config are configured

The grub configuration file contains information on boot settings and passwords for unlocking boot options.
Setting the permissions to read and write for root only prevents non-root users from seeing the boot parameters or changing them.
Non-root users who read the boot parameters may be able to identify security weaknesses at boot and exploit them.

```sh
chown root:root /boot/grub/grub.cfg
chmod 0400 /boot/grub/grub.cfg
echo 'DPkg::Post-Invoke {"chmod 600 /boot/grub/grub.cfg";};' >> /etc/apt/apt.conf.d/99permissions
```

mitre_mitigations: M1022  
mitre_tactics: TA0005, TA0007  
mitre_techniques: T1542  

## Ensure AppArmor is enabled in the bootloader configuration

AppArmor must be enabled at boot time in your bootloader configuration to ensure its controls are not overridden.

Edit `/etc/default/grub` and add the `apparmor=1` and `security=apparmor` parameters to the GRUB_CMDLINE_LINUX= line:

```
GRUB_CMDLINE_LINUX="apparmor=1 security=apparmor"
```

```sh
update-grub
```

mitre_mitigations: M1026  
mitre_tactics: TA0003  
mitre_techniques: T1068, T1565, T1565.001, T1565.003  

