# new.tf#!/bin/bash

echo "=============================================="
echo "        SERVER PERFORMANCE STATISTICS"
echo "=============================================="

# ----- OS Version -----
echo -e "\n🔹 OS Version:"
lsb_release -a 2>/dev/null || cat /etc/*release

# ----- Uptime -----
echo -e "\n🔹 Server Uptime:"
uptime -p
echo "Load Average:"
uptime | awk -F'load average:' '{print $2}'

# ----- Logged-in Users -----
echo -e "\n🔹 Logged-in Users:"
who

# ----- CPU Usage -----
echo -e "\n🔹 Total CPU Usage:"
top -bn1 | grep "Cpu(s)" | awk '{print "CPU Usage: "100 - $8"%"}'

# ----- Memory Usage -----
echo -e "\n🔹 Memory Usage:"
free -h
echo ""
free | awk '/Mem:/ {
    used=$3; free=$4; total=$2; 
    printf("Used: %.2f%%\n", used/total*100)
}'

# ----- Disk Usage -----
echo -e "\n🔹 Disk Usage:"
df -h --total | grep 'total'
echo ""
df --total | grep 'total' | awk '{print "Used: "$3", Free: "$4", Usage: "$5}'

# ----- Top 5 CPU Consuming Processes -----
echo -e "\n🔹 Top 5 Processes by CPU Usage:"
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -n 6

# ----- Top 5 Memory Consuming Processes -----
echo -e "\n🔹 Top 5 Processes by Memory Usage:"
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -n 6

# ----- Failed login attempts (optional) -----
echo -e "\n🔹 Failed Login Attempts:"
grep "Failed password" /var/log/auth.log 2>/dev/null | wc -l

echo -e "\n=============================================="
echo "         END OF REPORT"
echo "=============================================="
