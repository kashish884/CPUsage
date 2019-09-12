# CPUsage
Monitoring shell script
#!/bin/bash
PATHS="/"
HOSTNAME=$(hostname)
X=90
Y=95

mkdir -p hw1
LOGFILE=hw1/cpusage.csv
ALERTFILE=hw1/alert.csv
touch $LOGFILE
touch $ALERTFILE

for path in $PATHS
do
        load1=`uptime|awk '{print $10}'|awk -F. '{print $1}'`
        load5=`uptime|awk '{print $11}'|awk -F. '{print $1}'`
        load15=`uptime|awk '{print $12}'|awk -F. '{print $1}'`
        timestamp=`top -b -n 2 -d1 | grep "load average" | tail -n1 | awk '{print $3}'`
        CPULOAD=`top -b -n 2 -d1 | grep "load average" | tail -n1 | awk '{for(i=12;i<=14;++i)printf $i""FS ;print ""}'`
if [ -n  $X -a -n $Y ]; then
if [ "$load1" -ge "$X" ] ; then
echo " $timestamp WARNING - HIGH CPU Usage $CPULOAD " >> $ALERTFILE

exit 1

elif  [ "$load5" -ge "$Y" -a "$load1" -ge "$Y" ] ; then
echo  " $timestamp WARNING - Very HIGH CPU Usage $CPULOAD " >> $ALERTFILE

exit 2

else
echo " $timestamp $CPULOAD " >> $LOGFILE

exit 0
fi
fi
done
#END#
