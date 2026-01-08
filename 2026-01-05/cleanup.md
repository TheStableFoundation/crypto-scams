# Malware Remediation & Cleanup Guide

This guide details the steps taken to neutralize the **AMOS/Realst** infostealer (identified by the `xxxblyat` signature) and remove its persistence mechanisms.

---

## Phase 1: Immediate Process Termination

Before removing files, the running instances of the script must be killed to prevent them from "protecting" their own configuration files.

```bash
# Force-kill any process containing the malware signature
ps aux | grep "xxxblyat" | grep -v grep | awk '{print $2}' | xargs kill -9 2>/dev/null
```

## Phase 2: Disabling Persistence (LaunchAgents)

The malware uses macOS `launchd` to ensure it restarts automatically. You must unload the configuration from the background manager before deleting the physical file.

```bash
# Unload from User context
launchctl unload ~/Library/LaunchAgents/com.35591.plist 2>/dev/null

# Unload from System context
sudo launchctl unload /Library/LaunchDaemons/com.35591.plist 2>/dev/null
```

## Phase 3: File System Cleanup

Remove the malicious configuration files and the hidden metadata used by the bot to communicate with the `Command & Control (C2)` server.

1. Remove Launch Configurations

```bash

# Delete User-level persistence
rm -f ~/Library/LaunchAgents/com.35591.plist

# Delete System-level persistence
sudo rm -f /Library/LaunchDaemons/com.35591.plist
```

2. Remove Hidden Tracking Files

The malware stores unique identifiers and "last action" timestamps in hidden files within the user's home directory.

```bash
# Remove hidden bot metadata
rm -f ~/.botid ~/.chost ~/.username ~/.lastaction ~/.uninstalled
```

## Phase 4: Verification

Confirm that no malicious processes or persistence files remain.

```bash
# Verify no active processes (should return 0 results)
ps aux | grep -E "xxxblyat|speeka" | grep -v grep

# Verify files are deleted
ls -la ~/Library/LaunchAgents/com.35591.plist
ls -la ~/.botid
```

## Phase 5: Mandatory Post-Infection Actions

Because this malware is a dedicated `InfoStealer`, the technical removal is only the first step. The following actions must be taken on a clean device:

Session Revocation: Log into your primary Email (Gmail/Proton) and use the "Sign out of all other sessions" feature.

Telegram Security: Open Telegram on Mobile > Settings > Devices > Terminate All Other Sessions.

Crypto Wallets: If `MetaMask` or `Phantom` were installed, assume the "Vault" is compromised. Move funds to a new seed phrase immediately.

Credential Rotation: Change passwords for all financial exchanges and primary communication tools.
