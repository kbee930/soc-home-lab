# Round 01 — Shieldpoint Recovery Engagement Log

**Date:** 2026-11-04
**Red team:** Shawn (NToaster) — redteam2, attack box 10.10.10.200
**Blue team:** Kyle (Dolamight) — Splunk alerting OFF this round (baseline run)
**Target:** DC01 (10.10.10.10), shieldpoint.local, Windows Server 2022
**Objective:** Exfiltrate debtor PII
**Result:** FLAG CAPTURED — FLAG{shieldpoint_round1_npi_exposed}

---

## Executive Summary

No unauthenticated foothold existed on DC01 — the box was well hardened against
anonymous access (null SMB/LDAP/RPC all denied, no AS-REP roastable accounts, no
anonymous vectors on any port). Access was obtained by cracking the username
convention, enumerating valid users, and password-spraying the weak account
(bcarter) with a guess pattern matching the AI-generated build script. That
credential had READ access to the "Customer Information" SMB share, from which
the Q3 debtor export (synthetic PII) was exfiltrated.

Key finding: One domain user (bcarter) with a weak, guessable password
(Summer2024!) was enough to reach confidential customer data.

---

## Timeline

| Time (ET) | Action | Result | ATT&CK |
|-----------|--------|--------|--------|
| 12:05 | nmap -sC -sV of DC | DC01, shieldpoint.local, SMB signing required | T1595.001 |
| 12:07 | SMB null session | connects; RID/LSA enum denied | T1087.002 |
| 12:35 | kerbrute user enum | cracked convention = flast | T1087.002 |
| 12:37 | full flast enum | valid: bcarter, lreyes, smoore | T1087.002 |
| 12:38 | AS-REP roast | all have pre-auth, no hashes | T1558.004 |
| 12:52 | full TCP + UDP scan | no unauthenticated vector | T1595.001 |
| 13:17 | FOOTHOLD: bcarter / Summer2024! | VALID (verified) | T1110.003 / T1078 |
| 13:17 | --shares | READ on Customer Information | T1135 |
| 13:19 | EXFIL Q3_Debtor_Export__Flag.txt | debtor PII + flag pulled | T1048 |

---

## Credentials Obtained

- bcarter / Summer2024! — READ on Customer Information share (intended weak account)

## Valid Users

administrator, bcarter, lreyes, smoore (convention: flast)

---

## Blue Team Notes (what should have alerted)

1. Kerberos user enumeration — burst of AS-REQs / 4768 for non-existent users.
2. Password spraying — many Event ID 4625 across accounts, same source.
3. THE ONE SUCCESS IN THE NOISE — single 4624 for bcarter right after a wall of
   4625s. Most important detection of the round.
4. Anonymous enumeration — null-session SMB/LDAP/RPC connections.
5. Sensitive share access — bcarter reading Customer Information (4663).
6. Data exfiltration — large/unusual SMB read of the debtor export.

---

## Recommendations

1. Ban seasonal/company-themed passwords (Summer2024! is trivially guessable).
2. Enable account lockout — none observed during spraying.
3. Least privilege on Customer Information share — bcarter should not have READ.
4. Enable and tune alerting for the six detections above.

---

## Open for Round 2 (post-foothold)

- BloodHound domain map
- Full authenticated user dump (--users)
- Hunt path to Domain Admin
- Kyle turns on Splunk alerting, then hunts
