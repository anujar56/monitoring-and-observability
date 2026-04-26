#!/bin/bash

#simple bash script to export the uptime of the pc

while true
do
        up=$(uptime | awk '{print $3}' | cut -d ":" -f 1)
        METRICS=$(cat <<EOF
# HELP cpu_load CPU load average
# TYPE cpu_load gauge
uptime $up
EOF
)

        echo -e "HTTP/1.1 200 OK\nContent-Type: text/plain\n\n$METRICS" | nc -l -p 9100 -q 1

done
