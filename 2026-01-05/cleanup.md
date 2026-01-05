# 1. Unload the daemon so it stops running
launchctl unload ~/Library/LaunchAgents/com.35591.plist 2>/dev/null
sudo launchctl unload /Library/LaunchDaemons/com.35591.plist 2>/dev/null

# 2. Delete the actual file (check both locations just in case)
rm ~/Library/LaunchAgents/com.35591.plist 2>/dev/null
sudo rm /Library/LaunchDaemons/com.35591.plist 2>/dev/null


ps aux | grep xxxblyat
