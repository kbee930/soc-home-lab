# Round 01 — Shieldpoint Recovery Blue Team Report

**Date:** 2026-11-04
**Blue team:** Kyle — investigating via Splunk/Sysmon on DC01
**Red team:** Shawn — redteam2, attack box 10.10.10.200
**Target:** DC01 (10.10.10.10), shieldpoint.local, Windows Server 2022
**Objective:** Detect, investigate, and remediate the Round 1 engagement
**Result:** Breach confirmed, scope contained, root cause fixed

---

## Executive Summary

A password spray against three accounts (bcarter, lreyes, smoore) succeeded on one: bcarter, whose password (Summer2024!) was weak enough to fall after roughly 14,000 attempts. That account had READ access to the Customer Information share, and the red team reports exfiltrating the Q3 debtor export from it.

Key finding: the logon itself is well documented in Splunk, but file access on the share couldn't be independently confirmed, Object Access auditing wasn't enabled on it at the time.

---

## Timeline

| Time (ET) | Event | Source |
|---|---|---|
| 12:39 | Password spray begins against bcarter, lreyes, smoore | Splunk (4625) |
| 1:04 | First successful logon as bcarter | Splunk (4624) |
| 1:16 | Last successful logon as bcarter | Splunk (4624) |
| 1:17 | Red team reports foothold confirmed | Red team log |
| 1:19 | Red team reports file exfiltrated | Red team log |

Red team's reported times run 10-15 minutes behind what Splunk recorded, likely clock drift or a reused SMB session that didn't generate a new logon event.

---

## Scope of Compromise

No group membership changes (4728, 4732, 4756), no new accounts (4720), no changes to bcarter's own privileges (4738). Compromise stayed within bcarter's existing access, no escalation, no lateral movement observed.

---

## Red Team Corroboration

Red team's log shows ~35 minutes of recon before the spray (nmap, null session attempts, user enumeration, a failed AS-REP roast) between 12:05 and 12:38 PM. None of it shows up in Splunk, the default Sysmon config filters most network connection events, so this recon window is currently invisible.

Their own list of what should have alerted (Kerberos enumeration, the spray, the one successful login in the noise, anonymous enumeration, share access, exfil) matches what I found independently. Good cross-check between the two sides.

---

## Indicators of Compromise

- Source IP: 10.10.10.200
- Account compromised: bcarter
- Accounts targeted, not compromised: lreyes, smoore
- Logon Type: 3 (Network/SMB)
- Event IDs: 4625, 4624
- File reportedly accessed: Q3_Debtor_Export__Flag, on Customer Information share
- Attack window: 12:39 PM – 1:16 PM

Lab-specific IOCs, private IP space, fictional accounts. No file hashes or malware indicators, this was credential-based access, not malware.

---

## Remediation

- Account lockout policy enabled
- Stronger password requirements enforced
- File access auditing enabled on sensitive shares
- Splunk alert built for failed-logon bursts
- Sysmon config under review to capture network connections (currently filtered)

---

## MITRE ATT&CK Mapping

T1110.003 (Password Spraying).
Mitigations: M1036 (Account Use Policies), M1027 (Password Policies).
Detection: DET0487 (Distributed Password Spraying via Authentication Failures Across Multiple Accounts),
    AN1336, high volume of auth failures using a small password set across multiple accounts in a short window.

