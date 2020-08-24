> Operating System: Centos


# Ensure core dumps are restricted 

A core dump is the memory of an executable program. It is generally used to determine why a program aborted. It can also be used to glean confidential information from a core file. The system provides the ability to set a soft limit for core dumps, but this can be overridden by the user.

## Rationale:

Setting a hard limit on core dumps prevents users from overriding the soft variable. If core dumps are required, consider setting limits for user groups (see limits. conf (5) ). In addition, setting the fs .suid_dumpabie variable to 0 will prevent setuid programs from dumping core.

## Audit:

Run the following commands and verify output matches:

```shell
grep -E "A\s*\*\s+hard\s+core" /etc/security/limits.conf /etc/security/limits.d/*
hard core 0
sysctl fs.suid dumpable fs.suid dumpable = 0
grep "fs\.suid dumpable" /etc/sysctl.conf /etc/sysctl.d/* fs.suid dumpable = 0
```

Run the following command to check if systemd-coredump is installed:

```shell
systemctl is-enabled coredump.service
```

if enabled or disabled is returned systemd-coredump is installed

## Remediation:

Add the following line to `/etc/security/limits.conf` or a `/etc/security/limits.d/*`

file:

```shell
* hard core 0`
```

Set the following parameter in `/etc/syscti.conf` or a `/etc/syscti.d/*` file:

```shell
fs.suid dumpable = 0
```

Run the following command to set the active kernel parameter:

```shell
sysctl -w fs.suid dumpable=0`
```

If systemd-coredump is installed:

edit `/etc/systemd/coredump.conf` and add/modify the following lines:

```shell
Storage=none
ProcessSizeMax=0
```

Run the command:

`systemctl daemon-reload`



# Ensuring address space layout randomization (ASLR) is enabled 

Address space layout randomization (ASLR) is an exploit mitigation technique which randomly arranges the address space of key data areas of a process.

## Rationale:

Randomly placing virtual memory regions will make it difficult to write memory page exploits as the memory placement will be consistently shifting.

## Audit:

Run the following commands and verify output matches:

```shell
sysctl kernel.randomize va space kernel.randomize va space = 2
grep "kernel\.randomize va space" /etc/sysctl.conf /etc/sysctl.d/* kernel.randomize va space = 2 
```

## Remediation

Set the following parameter in `/etc/syscti.conf` or a `/etc/syscti.d/*` file:

```shell
kernel.randomize va space = 2
```

Run the following command to set the active kernel parameter:

```shell
sysctl -w kernel.randomize va space=2
```



















































