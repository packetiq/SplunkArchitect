---
output:
  html_document: default
  pdf_document: default
  word_document: default
---
# Disabling Transparent Huge Pages

### Introduction  

Some distributions of Linux (Red Hat/Cent OS and Ubuntu) have an advanced memory management scheme called __Transparent Huge Pages__ (THP). THP has been associated with degradation of Splunk Enterprise performance in at least some Linux kernel versions and should be disabled.  

More information about Splunk and Transparent Huge Pages can be found at these links:  

[Transparent huge memory pages and Splunk performance] (http://docs.splunk.com/Documentation/Splunk/6.5.2/ReleaseNotes/SplunkandTHP)

[How do I disable Transparent Huge Pages (THP) and confirm that it is disabled?] (https://answers.splunk.com/answers/188875/how-do-i-disable-transparent-huge-pages-thp-and-co.html)

Disable Huge pages command :
```
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```

[How to use, monitor, and disable transparent hugepages in Red Hat Enterprise Linux 6?] (https://access.redhat.com/solutions/46111)  


### Implementation  

Configure linux to start Splunk on boot:

```
sudo su  
<password>

./splunk enable boot-start
Init script installed at /etc/init.d/splunk.
Init script is configured to run at boot.  
```

The enable boot-start exercise above creates a 'splunk' file in /etc/rc.d/init.d

Edit this file to add functions to disable Transparent Huge Pages:  

__THIS IS EXISTING:__

```
splunk_status() {
  echo Splunk status:
  "/opt/splunk/bin/splunk" status
  RETVAL=$?
}
```

__ADD THIS FUNCTION UNDER THE ABOVE:__

```
# disable Transparent Huge Pages

disable_huge() {
  echo "disabling huge page support"
  if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
    echo never > /sys/kernel/mm/transparent_hugepage/enabled
  fi
  if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
    echo never > /sys/kernel/mm/transparent_hugepage/defrag
  fi
}
```

__FINALLY,  MODIFY THE   start & restart   CASES TO RUN THE disable_huge FUNCTION:__

```
case "$1" in
  start)
    disable_huge
    splunk_start
    ;;
  stop)
    splunk_stop
    ;;
  restart)
    disable_huge
    splunk_restart
    ;;
```

You can verify this script is working if:  

```/sys/kernel/mm/transparent_hugepage/enabled```
and 
```/sys/kernel/mm/transparent_hugepage/defrag```
contain:  
```always madvise [never]``` 

### Info on linux systemctl

https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units

You can run

```
systemctl status splunk.service -l
```

to check the status of the splunk startup sequence.

