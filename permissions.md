## Ensure permissions on bootloader config are configured

The grub configuration file contains information on boot settings and passwords for unlocking boot options.
Setting permissions to read and write for root only prevents non-root users from viewing or changing the boot parameters.
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

## Ensure permissions on /etc/ssh/sshd_config are configured

The `/etc/ssh/sshd_config` file, and files ending in `.conf` in the `/etc/ssh/sshd_config.d` directory, contain configuration specifications for `sshd`.
Configuration specifications for `sshd` need to be protected from unauthorized changes by non-privileged users.

```sh
chmod 0600 /etc/ssh/sshd_config.d/*.conf /etc/ssh/sshd_config
```

mitre_mitigations: M1022  
mitre_tactics: TA0005  
mitre_techniques: T1098, T1098.004, T1543, T1543.002  

## Ensure permissions on SSH public host key files are configured

An SSH public key is one of two files used in SSH public key authentication.
In this authentication method, a public key is a key that can be used for verifying digital signatures generated using a corresponding private key.
Only a public key that corresponds to a private key will successfully authenticate.
If an unauthorized user modifies a public host key file, the SSH service may be compromised.

```sh
chown root:root /etc/ssh/ssh_host_*_key
chmod 0600 /etc/ssh/ssh_host_*_key
chmod 0644 /etc/ssh/ssh_host_*_key.pub
```

mitre_mitigations: M1022  
mitre_tactics: TA0003, TA0006  
mitre_techniques: T1557  
