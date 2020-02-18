http://docs.splunk.com/Documentation/Splunk/6.5.2/Indexer/Backupindexeddata // link to splunk backup 
========Best practice ===================================================================================
//stat
curl -L https://bit.ly/glances | /bin/bash
=====================
//change port number Splunk web
/opt/splunk/bin/splunk set web-port 8080
or  //Configure Splunk Web to use SSL

====================
///disable huge pages Linux

echo never > /sys/kernel/mm/transparent_hugepage/enabled
====================
// Enumerate All SourceTypes for a specefic time and index

| metadata type=sourcetypes earliest=-7d index=main
====================
//List of All Hosts have sent data for a specefic index

| metadata type=hosts earliest=-7d index=main
====================
// forward internal data 
/opt/splunk/etc/system/local/outputs.conf

[indexAndForward]
index = false

[tcpout]
defaultGroup=IDX01
forwardedindex.filter.disable = true
indexAndForward = false

[tcpout:IDX01]
server=x.x.x.x:9997
====================
//permanently remove event data from all indexes 
/opt/splunk/bin/splunk clean eventdata
/opt/splunk/bin/splunk clean eventdata -index yourindex

====================
================
/// max searches 
$ sudo vim $SPLUNK_HOME/etc/system/local/limits.conf

[search]
base_max_searches=100
max_searches_per_cpu=10

================
///disable kvstore
[kvstore]
disabled = true
