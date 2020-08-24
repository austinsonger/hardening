> ***Operating System***: Centos
>
> Mandatory Access Control (MAC) provides an additional layer of access restrictions to processes on top of the base Discretionary Access Controls. By restricting how processes can access files and resources on a system the potential impact from vulnerabilities in the processes can be reduced.
>
> ***Impact***: Mandatory Access Control limits the capabilities of applications and daemons on a system, while this can prevent unauthorized access the configuration of MAC can be complex and difficult to implement correctly preventing legitimate access from occurring. 

# Configure SELinux

## 1.7.1.1	Ensure SELinux is installed 

SELinux provides Mandatory Access Control.

### Rationale:

Without a Mandatory Access Control system installed only the default Discretionary Access Control system will be available.

### Audit:

Verify SELinux is installed.

Run the following command:

```
rpm -q libselinux libselinux-<version>
```

### Remediation:

Run the following command to install SELinux:

```shell
dnf install libselinux
```



## Ensure SELinux is not disabled in bootloader configuration

Configure SELINUX to be enabled at boot time and verify that it has not been overwritten by the grub boot parameters.

### Rationale:

SELinux must be enabled at boot time in your grub configuration to ensure that the controls it provides are not overridden.

### Audit:

Run the following command and verify that no linux line has the seiinux=0 or enforcing=0 parameters set:

```shell
grep -E 'kernelopts=(\S+\s+)*(selinux=0|enforcing=0)+\b' /boot/grub2/grubenv
```

>  Nothing should be returned



### Remediation:

Edit `/etc/default/grub` and remove all instances of `selinux=0` and `enforcing=0` from all `CMDLINE_LINUX` parameters:

`GRUB_CMDLINE_LINUX_DEFAULT="quiet"`

`GRUB_CMDLINE_LINUX=""`

Run the following command to update the grub2 configuration:

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg
```



## Ensure SELinux policy is configured 

Configure SELinux to meet or exceed the default targeted policy, which constrains daemons and system software only.

### Rationale:

Security configuration requirements vary from site to site. Some sites may mandate a policy that is stricter than the default policy, which is perfectly acceptable. This item is intended to ensure that at least the default recommendations are met.

### Audit:

Run the following commands and ensure output matches either "targeted " or "mis ":

```shell
grep -E ,A\s*SELINUXTYPE=(targeted|mls)\b' /etc/selinux/config SELINUXTYPE=targeted
sestatus | grep Loaded
Loaded policy name:	targeted 
```

### Remediation:

Edit the `/etc/seiinux/config` file to set the `SELINUXTYPE` parameter:

`SELINUXTYPE=targeted`



## Ensure the SELinux state is enforcing 

Set SELinux to enable when the system is booted.

### Rationale:

SELinux must be enabled at boot time to ensure that the controls it provides are in effect at all times.

### Audit:

Run the following commands and ensure output matches:

```shell
grep -E ,A\s*SELINUX=enforcing' /etc/selinux/config SELINUX=enforcing

sestatus
SELinux status: enabled Current mode: enforcing Mode from config file: enforcing
```

### Remediation:

Edit the `/etc/selinux/config` file to set the `SELINUX` parameter:

`SELINUX=enforcing`

## Ensure no unconfined services exist 

Unconfined processes run in unconfined domains

### Rationale:

For unconfined processes, SELinux policy rules are applied, but policy rules exist that allow processes running in unconfined domains almost all access. Processes running in unconfined domains fall back to using DAC rules exclusively. If an unconfined process is compromised, SELinux does not prevent an attacker from gaining access to system resources and data, but of course, DAC rules are still used. SELinux is a security enhancement on top of DAC rules - it does not replace them.



### Audit:

Run the following command and verify not output is produced:

```shell
ps -eZ | grep unconfined service t
```



### Remediation:

Investigate any unconfined processes found during the audit action. They may need to have an existing security context assigned to them or a policy built for them.



## Ensure SETroubleshoot is not installed 

The SETroubleshoot service notifies desktop users of SELinux denials through a user- friendly interface. The service provides important information around configuration errors, unauthorized intrusions, and other potential errors.

### Rationale:

The SETroubleshoot service is an unnecessary daemon to have running on a server, especially if X Windows is disabled.



### Audit:

Verify `setroubieshoot` is not installed.

Run the following command:

```shell
 rpm -q setroubieshoot
 package setroubieshoot is not installed
```



### Remediation:

Run the following command to uninstall `setroubleshoot`:

```shell
dnf remove setroubleshoot
```

## Ensure the MCS Translation Service (mcstrans) is not installed 

The 	mcstransd	 daemon provides category label information to client processes requesting information. The label translations are defined in 	/etc/seiinux/targeted/setrans.conf	

### Rationale:

Since this service is not used very often, remove it to reduce the amount of potentially vulnerable code running on the system.

### Audit:

Verify `mcstrans` is not installed.

Run the following command:

```shell
rpm -q mcstrans
package mcstrans is not installed
```

### Remediation:

Run the following command to uninstall mcstrans:

```shell
dnf remove mcstrans
```
