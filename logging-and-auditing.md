> OPERATING SYSTEM: Centos

>It is recommended that rsyslog be used for logging (with logwatch providing summarization) and auditd be used for auditing (with aureportproviding summarization) to automatically monitor logs for intrusion attempts and other suspicious system behavior.
In addition to the local log files created by the steps in this section, it is also recommended that sites collect copies of their system logs on a secure, centralized log server via an encrypted connection. Not only does centralized logging help sites correlate events that may be occurring on multiple systems, but having a second copy of the system log information may be critical after a system compromise where the attacker has modified the local log files on the affected system(s). If a log correlation system is deployed, configure it to process the logs described in this section.

## Configure System Accounting

### Ensure auditing is enabled

#### Ensure auditd is installed 

#### Ensure auditd service is enabled 

#### Ensure auditing for processes that start prior to auditd is enabled 

#### Ensure audit_backlog_limit is sufficient 



### Configure Data Retention

#### Ensure audit log storage size is configured 

#### Ensure audit logs are not automatically deleted 

#### Ensure system is disabled when audit logs are full 

### Ensure changes to system administration scope (sudoers) is collected 

### Ensure login and logout events are collected

### Ensure session initiation information is collected 

### Ensure events that modify the system's Mandatory Access Controls are collected 

### Ensure events that modify the system's network environment are collected 

### Ensure discretionary access control permission modification events are collected 

### Ensure unsuccessful unauthorized file access attempts are collected 

### Ensure events that modify user/group information are collected 

### Ensure successful file system mounts are collected 

### Ensure use of privileged commands is collected 

### Ensure file deletion events by users are collected 

### Ensure kernel module loading and unloading is collected 

### Ensure system administrator actions (sudolog) are collected 

### Ensure the audit configuration is immutable 




## Configure Logging
> Logging services should be configured to prevent information leaks and to aggregate logs on a remote server so that they can be reviewed in the event of a system compromise and ease log analysis. 

### Configure rsyslog
> The rsyslog software is recommended as a replacement for thesysiogd daemon and provides improvements over sysiogd, such as connection-oriented (i.e. TCP) transmission of logs, the option to log to database formats, and the encryption of log data en route to a central logging server. Note: This section only applies if rsyslog is installed on the system. 

#### Ensure rsyslog is installed 

#### Ensure rsyslog Service is enabled 

#### Ensure rsyslog default file permissions configured 

#### Ensure logging is configured 

#### Ensure rsyslog is configured to send logs to a remote log host

#### Ensure remote rsyslog messages are only accepted on designated log hosts. 

### Configure journald 

#### Ensure journald is configured to send logs to rsyslog 

#### Ensure journald is configured to compress large log files 

#### Ensure journald is configured to write logfiles to persistent disk

### Ensure permissions on all logfiles are configured 

## Ensure logrotate is configured 
