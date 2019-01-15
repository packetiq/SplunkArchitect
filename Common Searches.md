
// List of All Hosts have not sent data past 24 hours

| metadata type=hosts
| eval "Last Recently"=now()-recentTime // (How many seconds ago the last log sent)

| search "Last Recently" > 86400 // (Filter just events related to before past day)

| rename totalCount as Count, host as Host, firstTime as "First Event", lastTime as "Last Event", recentTime as "Last Update", type as Type // (To be more readable titles)

| fieldformat "First Event"=strftime('First Event', "%c") // (To be more readable time)

| fieldformat "Last Event"=strftime('Last Event', "%c")

| fieldformat "Last Update"=strftime('Last Update', "%c")

| eval "Minutes Behind"=round('Last Recently'/60, 2) // (Calculates Minutes past from the last log sent)

| eval "Hours Behind"=round('Last Recently'/3600, 2) // Calculates Hours past from the last log sent)

| table Host, "First Event", "Last Event", "Last Update", "Minutes Behind", "Hours Behind" // (Make results as tabular view)

| sort - "Minutes Behind"
===================
// View the state of sending logs by time intervals (per 7 Hours)

| tstats count where sourcetype=stream:dns by _time, host span=7h

| xyseries _time, host, count
==================
// Identify hosts that are logging more or less data than might be normally expected

| tstats count by host, _time span=3h

| stats median(count) as median by host

| join host [| tstats count where earliest=-900d by host ]

| eval perc_diff=((count/median)*100)-100

| where perc_diff>700 OR perc_diff<30

| sort perc_diff

| rename median as "Median Event Count Past 2 years", count as "Event Count Past 1 year", perc_diff as "Perentage Difference"

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

========daily total by GB================================================================================

 index="_internal" source="*metrics.log" per_index_thruput | eval GB=kb/(1024*1024) | timechart span=1d sum(GB) | convert ctime(_time) as timestamp


========highest-usage indexes============================================================================

index="_internal" source="*metrics.log" per_index_thruput | eval GB=kb/(1024*1024) | stats sum(GB) as total by series date_mday | sort total | fields + date_mday,series,total | reverse

========Today's indexing by sourcetype====================================================================

index="_internal" source="*metrics.log" per_sourcetype_thruput | eval MB=kb/1024 | timechart span=10m sum(MB) by series

========Today's indexing by index=========================================================================

 index="_internal" source="*metrics.log" per_index_thruput | eval MB=kb/1024 | timechart span=10m sum(MB) by series
