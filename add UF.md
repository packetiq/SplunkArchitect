
cd $SPLUNK_HOME/bin


./splunk enable boot-start
./splunk add forward-server <host>:<port>
./splunk add monitor /var/log/syslog -sourcetype syslog -index linux_servers 
cd $SPLUNK_HOME/etc/system/local   outputs.conf
[monitor:///apache/*.log]
disabled = 0


https://splunkbase.splunk.com/app/1753/
https://splunkbase.splunk.com/app/3295/
