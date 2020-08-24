
> Operating System: Centos
>
> The recommendations in this section focus on securing the bootloader and settings involved in the boot process directly. 

# Ensuring permissions on bootloader config are configured

The grub configuration file contains information on boot settings and passwords for unlocking boot options. The grub configuration is usually `grub.cfg` and `grubenv` stored in/boot/grub2/'

## Rationale:

Setting the permissions to read and write for root only prevents non-root users from seeing the boot parameters or changing them. Non-root users who read the boot parameters may be able to identify weaknesses in security upon boot and be able to exploit them.

## Audit:

Run the following commands and verify `Uid` and `Gid` are both `0/roo`t and `Access` does not grant permissions to `group` or `other` :

```shell
#	stat /boot/grub2/grub.cfg
Access:	(0600/-rw	)	Uid:	(	0/	root)	Gid: (	0/	root)
#	stat /boot/grub2/grubenv
Access:	(0600/-rw	)	Uid:	(	0/	root)	Gid: (	0/	root) 
```

## Remediation:

Run the following commands to set permissions on your grub configuration:

```shell
#	chown root:root /boot/grub2/grub.cfg
#	chmod og-rwx /boot/grub2/grub.cfg
#	chown root:root /boot/grub2/grubenv
#	chmod og-rwx /boot/grub2/grubenv
```

This recommendation is designed around the grub bootloader, if LILO or another bootloader is in use in your environment enact equivalent settings.

Replace `/boot/grub2/grub.cfg` and `boot/grub2/grubenv` with the appropriate configuration file(s) for your environment


# Ensure bootloader password is set 

Setting the boot loader password will require that anyone rebooting the system must enter a password before being able to set command line boot parameters

## Rationale:

Requiring a boot password upon execution of the boot loader will prevent an unauthorized user from entering boot parameters or changing the boot partition. This prevents users from weakening security (e.g. turning off SELinux at boot time).

## Audit:

Run the following command:

```shell
grep "A\s*GRUB2_PASSWORD" /boot/grub2/user.cfg GRUB2 PASSWORD=<encrypted-password>
```

## Remediation:

Create an encrypted password with `grub2-setpassword` :

```shell
grub2-setpassword
Enter password: <password>
Confirm password: <password>
```

Run the following command to update the `grub2` configuration:

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg 
```

## Impact:

If password protection is enabled, only the designated superuser can edit a Grub 2 menu item by pressing "e" or access the GRUB 2 command line by pressing "c"

If GRUB 2 is set up to boot automatically to a password-protected menu entry the user has no option to back out of the password prompt to select another menu entry. Holding the SHIFT key will not display the menu in this case. The user must enter the correct username and password. If unable, the configuration files will have to be edited via the LiveCD or other means to fix the problem

You can add --unrestricted to the menu entries to allow the system to boot without entering a password. Password will still be required to edit menu items.

## Notes:

This recommendation is designed around the grub2 bootloader, if LILO or another bootloader is in use in your environment enact equivalent settings.

Replace /boot/grub2/grub.cfg with the appropriate grub configuration file for your environment

The superuser/user information and password do not have to be contained in the /etc/grub.d/00_header file. The information can be placed in any /etc/grub.d file as long as that file is incorporated into grub.cfg. The user may prefer to enter this data into a custom file, such as /etc/grub.d/40_custom so it is not overwritten should the Grub package be updated. If placing the information in a custom file, do not include the "cat << EOF" and "EOF" lines as the content is automatically added from these files.

# Ensure authentication required for single user mode

Single user mode (rescue mode) is used for recovery when the system detects an issue during boot or by manual selection from the bootloader.

## Rationale:

Requiring authentication in single user mode (rescue mode) prevents an unauthorized user from rebooting the system into single user to gain root privileges without credentials.

## Audit:

Run the following commands and verify that `/sbin/suiogin` or `/usr/sbin/suiogin` is used as shown:

```shell
grep /systemd-sulogin-shell /usr/lib/systemd/system/rescue.service ExecStart=-/usr/lib/systemd/systemd-sulogin-shell rescue
grep /systemd-sulogin-shell /usr/lib/systemd/system/emergency.service ExecStart=-/usr/lib/systemd/systemd-sulogin-shell emergency 
```

## Remediation:

Edit `/usr/lib/systemd/system/rescue.service` and add/modify the following line: `ExecStart=-/usr/lib/systemd/systemd-sulogin-shell rescue`

Edit `/usr/lib/systemd/system/emergency.service` and add/modify the following line: `ExecStart=-/usr/lib/systemd/systemd-sulogin-shell emergency`

















