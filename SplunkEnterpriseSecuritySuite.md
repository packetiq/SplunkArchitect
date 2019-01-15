

# Note that because the UNIX app uses a single script to retrieve information
# from multiple OS flavors, and is intended to run on Universal Forwarders,
# it is not possible to differentiate between OS flavors by assigning
# different sourcetypes for each OS flavor (e.g. Linux:SSHDConfig), as was
# the practice in the older deployment-apps included with ES. Instead, 
# sourcetypes are prefixed with the generic "Unix".

# May require Splunk forwarder to run as root on some platforms.
[script://./bin/openPortsEnhanced.sh]
disabled = true
interval = 3600
source = Unix:ListeningPorts
sourcetype = Unix:ListeningPorts

[script://./bin/passwd.sh]
disabled = true
interval = 3600
source = Unix:UserAccounts
sourcetype = Unix:UserAccounts

# Only applicable to Linux
[script://./bin/selinuxChecker.sh]
disabled = true
interval = 3600
source = Linux:SELinuxConfig
sourcetype = Linux:SELinuxConfig

# Currently only supports SunOS, Linux, OSX.
# May require Splunk forwarder to run as root on some platforms.
[script://./bin/service.sh]
disabled = true
interval = 3600
source = Unix:Service
sourcetype = Unix:Service

# Currently only supports SunOS, Linux, OSX.
# May require Splunk forwarder to run as root on some platforms.
[script://./bin/sshdChecker.sh]
disabled = true
interval = 3600
source = Unix:SSHDConfig
sourcetype = Unix:SSHDConfig

# Currently only supports Linux, OSX.
# May require Splunk forwarder to run as root on some platforms.
[script://./bin/update.sh]
disabled = true
interval = 86400
source = Unix:Update
sourcetype = Unix:Update

[script://./bin/uptime.sh]
disabled = true
interval = 86400
source = Unix:Uptime
sourcetype = Unix:Uptime

[script://./bin/version.sh]
disabled = true
interval = 86400
source = Unix:Version
sourcetype = Unix:Version

# This script may need to be modified to point to the VSFTPD configuration file.
[script://./bin/vsftpdChecker.sh]
disabled = true
interval = 86400
source = Unix:VSFTPDConfig
sourcetype = Unix:VSFTPDConfig
