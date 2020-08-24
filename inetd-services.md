> Operating System: Centos
>
> inetd is a super-server daemon that provides internet services and passes connections to configured services. While not commonly used inetd and any unneeded inetd based services should be disabled if possible.
>

## Ensure xinetd is not installed 

The extended InterNET Daemon ( xinetd ) is an open source super daemon that replaced the original inetd daemon. The xinetd daemon listens for well known services and dispatches the appropriate daemon to properly respond to service requests.

### Rationale:

If there are no xinetd services required, it is recommended that the package be removed.

### Audit:

Run the following command to verify xinetd is not installed:

```shell
rpm -q xinetd
package xinetd is not installed
```

### Remediation:

Run the following command to remove xinetd:

```shell
dnf remove xinetd
```

