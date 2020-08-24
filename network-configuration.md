> Operating System: Centos
> 
> This section provides guidance on for securing the network configuration of the system through kernel parameters, access list control, and firewall settings. 
> 

## Network Parameters 
> The following network parameters are intended for use if the system is to act as a host only. A system is considered host only if the system has a single interface, or has multiple interfaces but will not be configured as a router. 


### Ensure IP forwarding is disabled 

The net.ipv4.ip forward and net.ipv6.conf.all.forwarding flags are used to tell the system whether it can forward packets or not.

#### Rationale:

Setting the flags to 0 ensures that a system with multiple interfaces (for example, a hard proxy), will never be able to forward packets, and therefore, never serve as a router.

#### Audit:

Run the following command and verify output matches:

```shell
sysctl net.ipv4.ip forward net.ipv4.ip forward = 0
grep -E -s "A\s*net\.ipv4\.ip forward\s*=\s*1" /etc/sysctl.conf /etc/sysctl.d/*.conf /usr/lib/sysctl.d/*.conf /run/sysctl.d/*.conf
No value should be returned

sysctl net.ipv6.conf.all.forwarding net.ipv6.conf.all.forwarding = 0
grep -E -s "A\s*net\.ipv6\.conf\.all\.forwarding\s*=\s*1" /etc/sysctl.conf /etc/sysctl.d/*.conf /usr/lib/sysctl.d/*.conf /run/sysctl.d/*.conf
No value should be returned 
```



#### Remediation

Run the following commands to restore the default parameters and set the active kernel parameters:

```shell
grep -Els "A\s*net\.ipv4\.ip forward\s*=\s*1" /etc/sysctl.conf /etc/sysctl.d/*.conf /usr/lib/sysctl.d/*.conf /run/sysctl.d/*.conf | while read filename; do sed -ri "s/A\s*(net\.ipv4\.ip forward\s*)(=)(\s*\S+\b).*$/# *REMOVED* \1/" $filename; done; sysctl -w net.ipv4.ip forward=0; sysctl -w net.ipv4.route.flush=1

grep -Els "A\s*net\.ipv6\.conf\.all\.forwarding\s*=\s*1" /etc/sysctl.conf /etc/sysctl.d/*.conf /usr/lib/sysctl.d/*.conf /run/sysctl.d/*.conf | while read filename; do sed -ri
"s/A\s*(net\.ipv6\.conf\.all\.forwarding\s*)(=)(\s*\S+\b).*$/# *REMOVED* \1/" $filename; done; sysctl -w net.ipv6.conf.all.forwarding=0; sysctl -w net.ipv6.route.flush=1
```



### Ensure packet redirect sending is disabled 



## Network Parameters (Host and Router)

> The following network parameters are intended for use on both host only and router systems. A system acts as a router if it has at least two interfaces and is configured to perform routing functions.



### Ensure source routed packets are not accepted 



### Ensure ICMP redirects are not accepted 



### Ensure secure ICMP redirects are not accepted 



### Ensure suspicious packets are logged 



### Ensure broadcast ICMP requests are ignored 



### Ensure bogus ICMP responses are ignored 



### Ensure Reverse Path Filtering is enabled 



### Ensure TCP SYN Cookies is enabled 



### Ensure IPv6 router advertisements are not accepted 



## Uncommon Network Protocols

> The Linux kernel modules support several network protocols that are not commonly used. If these protocols are not needed, it is recommended that they be disabled in the kernel.



### Ensure DCCP is disabled 

### Ensure SCTP is disabled 

### Ensure RDS is disabled 

### Ensure TIPC is disabled 



## Firewall Configuration

> A firewall Provides defense against external and internal threats by refusing unauthorized connections, to stop intrusion and provide a strong method of access control policy.



### Ensure a Firewall package is installed 







## Configure firewalld

> firewalld (Dynamic Firewall Manager) provides a dynamically managed firewall with support for network/firewall “zones” to assign a level of trust to a network and its associated connections, interfaces or sources. It has support for IPv4, IPv6, Ethernet bridges and also for IPSet firewall settings. There is a separation of the runtime and permanent configuration options. It also provides an interface for services or applications to add iptables, ip6tables and ebtables rules directly. This interface can also be used by advanced users.



### Ensure firewalld service is enabled and running 



### Ensure nftables is not enabled 



### Ensure default zone is set 



### Ensure network interfaces are assigned to appropriate zone 



### Ensure unnecessary services and ports are not accepted 



### Ensure iptables is not enabled 





## Configure nftables

> nftables is a subsystem of the Linux kernel providing filtering and classification of network packets/datagrams/frames and is the successor to iptables. The biggest change with the successor nftables is its simplicity. With iptables, we have to configure every single rule and use the syntax which can be compared with normal commands. With nftables, the simpler syntax, much like BPF (Berkely Packet Filter) means shorter lines and less repetition. Support for nftables should also be compiled into the kernel, together with the related nftables modules. Please ensure that your kernel supports nf_tables before choosing this option.



### Ensure iptables are flushed 



### Ensure a table exists 



### Ensure base chains exist 



### Ensure loopback traffic is configured 



### Ensure outbound and established connections are configured 



### Ensure default deny firewall policy 



### Ensure nftables service is enabled 



### Ensure nftables rules are permanent 



## Configure iptables

> IPtables is an application that allows a system administrator to configure the IPv4 and IPv6 tables, chains and rules provided by the Linux kernel firewall. While several methods of configuration exist this section is intended only to ensure the resulting IPtables rules are in place, not how they are configured. If IPv6 is in use in your environment, similar settings should be applied to the IP6tables as well.



### Configure IPv4 iptables

> Iptables is used to set up, maintain, and inspect the tables of IP packet filter rules in the Linux kernel. Several different tables may be defined. Each table contains a number of built-in chains and may also contain user-defined chains.





#### Ensure default deny firewall policy 



#### Ensure loopback traffic is configured 



#### Ensure outbound and established connections are configured 



#### Ensure firewall rules exist for all open ports 





### Configure IPv6 ip6tables

> Ip6tables is used to set up, maintain, and inspect the tables of IPv6 packet filter rules in the Linux kernel. Several different tables may be defined. Each table contains a number of built-in chains and may also contain user-defined chains. Each chain is a list of rules which can match a set of packets. Each rule specifies what to do with a packet that matches. This is called a 'target', which may be a jump to a user-defined chain in the same table.




#### Ensure IPv6 default deny firewall policy 



#### Ensure IPv6 loopback traffic is configured 



#### Ensure IPv6 outbound and established connections are configured 



#### Ensure IPv6 firewall rules exist for all open ports 





## Ensure wireless interfaces are disabled 





## Disable IPv6 


