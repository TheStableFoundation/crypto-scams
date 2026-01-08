# Crypto Scams & Malware Analysis

A repository for documenting active crypto-targeted scams and malware for educational and defensive purposes. Developed in collaboration with [smbCloud](https://github.com/smbcloudXYZ).

---

## Safety Disclaimer

The information in this repository is for **learning and incident response purposes only**. Interacting with the links or commands documented here on a production machine is extremely dangerous and will result in the loss of assets and personal data. Always use a sandboxed environment for analysis.

---

## Active Incident Reports

### 1. Executive Impersonation: "Maven 11 / Mathijs" (Jan 2026)

**Type:** Social Engineering + macOS InfoStealer (AMOS/Realst)

#### Incident Summary

An attacker impersonates **Mathijs van Esch** (General Partner at Maven 11) on Telegram. The attacker uses high-pressure social engineering to invite victims to a fake "exclusive" platform.

- **Phishing Domain:** `https://speeka.app`
- **Malware Payload:** A bash one-liner executed via Terminal.
- **Signature:** Internal script ID `xxxblyat`.
- **Impact:** Instant exfiltration of browser cookies, Telegram session tokens, and crypto wallet "vault" files.

#### Indicators of Compromise (IoCs)

- **Malicious URL:** `https://macos.speeka.app/apple/macos/installation/terminal/launcher`
- **Persistence File:** `com.35591.plist`
- **Hidden Metadata Files:** `~/.botid`, `~/.chost`, `~/.username`

---

## Remediation Resources

For detailed steps on how to identify and remove this specific malware, refer to the following documentation:

- [**Malware Cleanup Guide (cleanup.md)**](./cleanup.md) - Step-by-step technical removal of the "xxxblyat" stealer.

---

## How to Contribute

If you have encountered a scam or have forensic data from an attack:

1. Open an Issue with the scam name and date.
2. Provide sanitized logs or screenshots of the communication.
3. Do **not** upload live malware binaries; only provide links or scripts in a text-based, non-executable format.

---

## LICENSE

This project is licensed under the [MIT License](./LICENSE).
