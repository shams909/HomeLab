#For Telegram Automation

 ##First, Create the Scripts Directory
`mkdir -p ~/scripts`

`Telegram Alert`
cat > ~/scripts/telegram-alert.sh << 'EOF'
#!/bin/bash

#Your Telegram Bot Token (get from @BotFather)
TELEGRAM_BOT_TOKEN="YOUR_BOT_TOKEN_HERE"

#Your Chat ID (message your bot, then visit: https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates)
TELEGRAM_CHAT_ID="YOUR_CHAT_ID_HERE"

#The message to send
MESSAGE="$1"

#Send message via Telegram API
curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
     -d "chat_id=$TELEGRAM_CHAT_ID&text=$MESSAGE" > /dev/null
EOF

Make it executable:

chmod +x ~/scripts/telegram-alert.sh


`System Health`
#!/bin/bash

# Create the scripts directory if it doesn't exist
mkdir -p ~/scripts/

# Create the system health script with proper EOF formatting
cat > ~/scripts/system-health.sh << 'EOF'
#!/bin/bash
#System & Docker Health Report
echo "🔄 Running system health check..."
DATE=$(date "+%Y-%m-%d %I:%M:%S %p")
REPORT="🏠 System & Docker - $DATE
────────────────
🖥️ SYSTEM
Uptime: $(uptime -p | sed 's/up //')
Disk: $(df -h / | awk 'NR==2{print $4 " free (" $5 " used)"}')
Memory: $(free -h | awk 'NR==2{print $3 " / " $2}')
Load: $(uptime | awk -F'load average:' '{print $2}' | tr -d ' ')

🐳 DOCKER  
Service: $(systemctl is-active docker)
Containers: $(docker ps -q | wc -l) running

🌐 NETWORK
Internet: $(ping -c 2 -W 3 1.1.1.1 >/dev/null 2>&1 && echo "✅ Connected" || echo "❌ Disconnected")
Power: $(if command -v acpi &>/dev/null && acpi -b &>/dev/null; then echo "🔋 $(acpi -b | grep -P -o '[0-9]+(?=%)' | head -1)%"; else echo "🔌 AC Connected"; fi)
────────────────
✅ System optimal"

~/scripts/telegram-alert.sh "$REPORT"
echo "✅ System health check completed!"
EOF

# Make the script executable
chmod +x ~/scripts/system-health.sh

echo "✅ Script created successfully at ~/scripts/system-health.sh"
`Battery Monitor Script`
cat > ~/scripts/battery-monitor.sh << 'EOF'
#!/bin/bash

# Get battery percentage
BATTERY_INFO=$(acpi -b 2>/dev/null)
if [ $? -ne 0 ]; then
    # acpi command failed (probably not available or no battery)
    exit 0
fi

BATTERY_LEVEL=$(echo "$BATTERY_INFO" | grep -P -o '[0-9]+(?=%)' | head -1)

# Check if we got a valid number
if ! [[ "$BATTERY_LEVEL" =~ ^[0-9]+$ ]]; then
    exit 0
fi

# Check battery level and send alerts
if [ "$BATTERY_LEVEL" -le 20 ]; then
    # Battery critical - send alert and shutdown
    ~/scripts/telegram-alert.sh "🔋 CRITICAL: Battery at $BATTERY_LEVEL%. Shutting down homelab safely in 1 minute!"
    shutdown -h +1
elif [ "$BATTERY_LEVEL" -le 30 ]; then
    # Battery warning
    ~/scripts/telegram-alert.sh "⚠️ Battery low: $BATTERY_LEVEL%. Connect to power soon."
elif [ "$BATTERY_LEVEL" -le 50 ]; then
    # Battery info
    ~/scripts/telegram-alert.sh "🔋 Battery: $BATTERY_LEVEL% remaining."
fi
EOF

Make it executable:

chmod +x ~/scripts/battery-monitor.sh

`Internet Monitor`
cat > ~/scripts/internet-monitor.sh << 'EOF'
#!/bin/bash

# Check internet connectivity
if ! ping -c 2 -W 5 1.1.1.1 > /dev/null 2>&1; then
    ~/scripts/telegram-alert.sh "🌐 INTERNET DOWN: No connection to internet!"
    exit 1
fi

# Check if we can resolve DNS
if ! nslookup google.com > /dev/null 2>&1; then
    ~/scripts/telegram-alert.sh "🌐 DNS FAILURE: Cannot resolve domain names!"
    exit 1
fi
EOF

Make it executable:

chmod +x ~/scripts/internet-monitor.sh


`service monitor script`
cat > ~/scripts/service-monitor.sh << 'EOF'
#!/bin/bash

# Check if AdGuard Home is running
if ! systemctl is-active AdGuardHome >/dev/null 2>&1; then
    ~/scripts/telegram-alert.sh "🛑 AdGuard Home DOWN! Restarting..."
    sudo systemctl restart AdGuardHome
fi

# Check if Docker is running
if ! systemctl is-active docker >/dev/null 2>&1; then
    ~/scripts/telegram-alert.sh "🐳 DOCKER DOWN! Restarting..."
    sudo systemctl restart docker
fi
EOF

Make it executable 
chmod +x ~/scripts/service-monitor.sh

#Set Your Timezone on the Ubuntu VM to get the tie sync to ur local time

# Set timezone to UTC(According to ur location)
sudo timedatectl set-timezone Asia/___  # or Asia/_any_ depending on your location

# Verify timezone
timedatectl
date


`Daily Health check scripts`

cat > ~/scripts/daily-health-check.sh << 'EOF'
#!/bin/bash

# Health Check Report
echo "🔄 Running health check..."

# Get current date in 12-hour format with AM/PM (local time)
DATE=$(date "+%Y-%m-%d %I:%M:%S %p")

# Build report with clear separation
REPORT="🏠 Homelab Status - $DATE
────────────────
🖥️ SYSTEM
Uptime: $(uptime -p | sed 's/up //')
Disk: $(df -h / | awk 'NR==2{print $4 " free (" $5 " used)"}')
Memory: $(free -h | awk 'NR==2{print $3 " / " $2}')
Load: $(uptime | awk -F'load average:' '{print $2}' | tr -d ' ')

🐳 DOCKER  
Service: $(systemctl is-active docker)
Containers: $(docker ps -q | wc -l) running

🛡️ ADGUARD HOME (Native)
Service: $(systemctl is-active AdGuardHome >/dev/null 2>&1 && echo "✅ Running" || echo "❌ Stopped")

🌐 NETWORK
Internet: $(ping -c 2 -W 3 1.1.1.1 >/dev/null 2>&1 && echo "✅ Connected" || echo "❌ Disconnected")
Power: $(if command -v acpi &>/dev/null && acpi -b &>/dev/null; then echo "🔋 $(acpi -b | grep -P -o '[0-9]+(?=%)' | head -1)%"; else echo "🔌 AC Connected"; fi)
────────────────
✅ All systems optimal!
Next update: $(date -d "+3 hours" "+%I:%M %p")"

# Send the report
~/scripts/telegram-alert.sh "$REPORT"

echo "✅ Health check completed!"
EOF

🚀 Make executable and test:

chmod +x ~/scripts/daily-health-check.sh
~/scripts/daily-health-check.sh

`The Confirmation Message Script`
# Send  confirmation with proper time format
~/scripts/telegram-alert.sh "✅ All monitoring systems activated! 
• Battery checks every 5 min
• Internet checks every 10 min  
• Service checks every 15 min
• Full health reports every 3 hours
Next health report: $(date -d "+3 hours" "+%I:%M %p")"



`Now the output will be somthing like that :`
🏠 Homelab Status - 2025-08-28 06:00:02 PM
────────────────
🖥️ SYSTEM
Uptime: 13 hours, 25 minutes
Disk: 62G free (13% used)
Memory: 569Mi / 7.8Gi
Load: 0.00,0.00,0.00

🐳 DOCKER
Service: active
Containers: 2 running

🛡️ ADGUARD HOME (Native)  
Service: ✅ Running

🌐 NETWORK
Internet: ✅ Connected
Power: 🔌 AC Connected
────────────────
✅ All systems optimal!
Next update: 09:00 PM


#Setup Cron Jobs (Automated Monitoring)
crontab -l > /tmp/mycron 2>/dev/null

# Add the monitoring jobs
echo "*/5 * * * * ~/scripts/battery-monitor.sh" >> /tmp/mycron
echo "*/10 * * * * ~/scripts/internet-monitor.sh" >> /tmp/mycron
echo "*/15 * * * * ~/scripts/service-monitor.sh" >> /tmp/mycron

# Install the new cron file
crontab /tmp/mycron
rm /tmp/mycron

# Add to Crontab for Daily Execution
# Add to crontab to run every day at 9:00 AM
(crontab -l 2>/dev/null; echo "0 9 * * * ~/scripts/daily-health-check.sh") | crontab -

# Or choose your preferred time:
# 0 9 * * *  = 9:00 AM daily
# 0 18 * * * = 6:00 PM daily
# 0 12 * * * = 12:00 PM daily

Test It Immediately

# Run the health check to make sure it works
~/scripts/daily-health-check.sh
#Optional: Add Weekly Summary
# Create a weekly summary script
cat > ~/scripts/weekly-summary.sh << 'EOF'
#!/bin/bash
WEEKLY_REPORT="📅 *Weekly Homelab Summary* - $(date "+%Y-%m-%d")
----------------------------------------
This week your homelab served: 
- 🛡️ Millions of DNS queries
- ⚡ Protected all devices from ads
- 💾 Ran 24/7 without issues
----------------------------------------
Keep up the great work! 🚀"

~/scripts/telegram-alert.sh "$WEEKLY_REPORT"
EOF

chmod +x ~/scripts/weekly-summary.sh

# Run every Sunday at 10:00 AM
(crontab -l 2>/dev/null; echo "0 10 * * 0 ~/scripts/weekly-summary.sh") | crontab -


⚠️ IMPORTANT: Configure Your Telegram Bot
Message @BotFather on Telegram

Send /newbot and follow instructions

Get your BOT_TOKEN

Message your new bot, then visit:
https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates

Find your CHAT_ID in the response

Edit ~/scripts/telegram-alert.sh and replace:

YOUR_BOT_TOKEN_HERE with your actual token

YOUR_CHAT_ID_HERE with your actual chat ID

#Also ad Adguard dedicated monitoring if u want

adguard-monitor.sh
#!/bin/bash

# Dedicated AdGuard Home Monitoring
echo "🔄 Checking AdGuard Home..."

# Get resource usage
AG_PID=$(pgrep AdGuardHome)
if [ -n "$AG_PID" ]; then
    AG_CPU=$(top -bn1 -p $AG_PID | tail -1 | awk '{print $9}')
    AG_MEM=$(top -bn1 -p $AG_PID | tail -1 | awk '{print $10}')
    AG_STATUS=$(systemctl is-active AdGuardHome)
else
    AG_CPU="N/A"
    AG_MEM="N/A"
    AG_STATUS="inactive"
fi

REPORT="🛡️ AdGuard Home - $(date "+%I:%M %p")
────────────────
Status: $AG_STATUS
CPU: ${AG_CPU}% | Memory: ${AG_MEM}%

📊 Live Stats:
Queries: $(curl -s http://localhost:3000/control/stats | grep -o '"dns_queries":[0-9]*' | cut -d: -f2)
Blocked: $(curl -s http://localhost:3000/control/stats | grep -o '"blocked_filtering":[0-9]*' | cut -d: -f2)
Blocked %: $(curl -s http://localhost:3000/control/stats | grep -o '"blocked_percentage":[0-9.]*' | cut -d: -f2)%

────────────────
$(if [ "$AG_STATUS" = "active" ]; then echo "✅ Service healthy"; else echo "❌ Service down"; fi)"

~/scripts/telegram-alert.sh "$REPORT"
echo "✅ AdGuard monitoring completed!"

#Note
`System Load: 0.00, 0.00, 0.00`
🎉 It means your system is:

Not overloaded

Running smoothly

Has plenty of CPU headroom

Load average shows:

0.00 = system is idle

1.00 = CPU is 100% utilized

4.00 on a 4-core CPU = fully loaded

0.00 means your homelab is running perfectly with no strain!
