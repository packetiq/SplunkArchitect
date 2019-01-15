# Copyright (C) 2018 Splunk Inc. All Rights Reserved.
[script://./bin/vmstat.sh]
interval = 60
sourcetype = vmstat
source = vmstat
disabled = 0

[script://./bin/iostat.sh]
interval = 60
sourcetype = iostat
source = iostat
disabled = 0

[script://./bin/ps.sh]
interval = 30
sourcetype = ps
source = ps
disabled = 0

[script://./bin/top.sh]
interval = 60
sourcetype = top
source = top
disabled = 0

[script://./bin/netstat.sh]
interval = 60
sourcetype = netstat
source = netstat
disabled = 0

[script://./bin/bandwidth.sh]
interval = 60
sourcetype = bandwidth
source = bandwidth
disabled = 0

[script://./bin/protocol.sh]
interval = 60
sourcetype = protocol
source = protocol
disabled = 0

[script://./bin/openPorts.sh]
interval = 300
sourcetype = openPorts
source = openPorts
disabled = 0

[script://./bin/time.sh]
interval = 21600
sourcetype = time
source = time
disabled = 0

[script://./bin/lsof.sh]
interval = 600
sourcetype = lsof
source = lsof
disabled = 0

[script://./bin/df.sh]
interval = 300
sourcetype = df
source = df
disabled = 0

# Shows current user sessions
[script://./bin/who.sh]
sourcetype = who
source = who
interval = 150
disabled = 0

# Lists users who could login (i.e., they are assigned a login shell)
[script://./bin/usersWithLoginPrivs.sh]
sourcetype = usersWithLoginPrivs
source = usersWithLoginPrivs
interval = 3600
disabled = 0

# Shows last login time for users who have ever logged in
[script://./bin/lastlog.sh]
sourcetype = lastlog
source = lastlog
interval = 300
disabled = 0

# Shows stats per link-level Etherner interface (simply, NIC)
[script://./bin/interfaces.sh]
sourcetype = interfaces
source = interfaces
interval = 60
disabled = 0

# Shows stats per CPU (useful for SMP machines)
[script://./bin/cpu.sh]
sourcetype = cpu
source = cpu
interval = 30
disabled = 0

# This script reads the auditd logs translated with ausearch
[script://./bin/rlog.sh]
sourcetype = auditd
source = auditd
interval = 60
disabled = 0

# Run package management tool collect installed packages
[script://./bin/package.sh]
sourcetype = package
source = package
interval = 3600
disabled = 0

[script://./bin/hardware.sh]
sourcetype = hardware
source = hardware
interval = 36000
disabled = 0

[monitor:///Library/Logs]
disabled = 0

[monitor:///var/log]
whitelist=(\.log|log$|messages|secure|auth|mesg$|cron$|acpid$|\.out)
blacklist=(lastlog|anaconda\.syslog)
disabled = 0

[monitor:///var/adm]
whitelist=(\.log|log$|messages)
disabled = 0

[monitor:///etc]
whitelist=(\.conf|\.cfg|config$|\.ini|\.init|\.cf|\.cnf|shrc$|^ifcfg|\.profile|\.rc|\.rules|\.tab|tab$|\.login|policy$)
disabled = 0

### bash history
[monitor:///root/.bash_history]
disabled = true
sourcetype = bash_history

[monitor:///home/*/.bash_history]
disabled = true
sourcetype = bash_history
