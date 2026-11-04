# Rules of Engagement — Shieldpoint Recovery Lab

## Engagement Brief
- Client: Shieldpoint Recovery (fictional debt collection agency)
- Type: External/internal network pentest, assumed weak security posture
- Objective: Demonstrate impact (e.g., access to debtor PII, domain compromise)
- Data: All synthetic
- Red team: redteam2 (Shawn), from attack box 100.75.224.17
- In scope: <lab subnet, TBD from Kyle>
- Out of scope: Kyle's home LAN, personal devices, router, internet, Tailscale admin
- Window: <start> – <end>
- Stop word: RED DRAGON

**Version:** 1.0
**Effective date:** [11/2/2026]
**Red team:** [Shawn/ NToaster]
**Blue team:** [Kyle/ Dolamight]

This is a consensual, educational security lab between two participants.
Both parties agree to the rules below before any engagement begins.

## 1. Scope

**In scope (authorized targets):**
- Windows Server 2022 Domain Controller — shieldpoint.local — [LAB IP]
- [Any other lab VMs, listed by hostname + IP]
- The isolated internal lab network only: [e.g. 10.0.0.0/24]

**Out of scope (never touch):**
- The host PC running VirtualBox (either participant's)
- Any real home network, router, Wi-Fi, or internet-facing service
- Any real accounts (email, cloud, banking, social)
- Any device not explicitly listed as a lab VM

Red team connects only through the provided VPN into the attacker VM
and does not pivot, scan, or route outside the lab network.

## 2. Off-Limits Actions

Even against in-scope targets, the following are prohibited:
- Anything that risks the host machine (VM escape, filling host disk,
  tampering with VirtualBox or host config)
- Deploying real malware, ransomware, or live C2 against anything —
  use harmless, clearly-labeled simulations only
- Destroying logs or evidence the blue team needs to investigate
  (unless log-tampering is the agreed objective for that round, and
  is documented)
- Social engineering real people outside the lab

## 3. Stop Word

If either participant sends the stop word, ALL activity halts
immediately — no questions, no "one more thing."

**Stop word:** `RED DRAGON`

Use it if anything touches something real, the host misbehaves, data
looks like it might not be synthetic, or anyone's just not comfortable.

## 4. Timing

- Attacks may occur: [any time / only during WINDOW, e.g. "Fri–Sun"]
- Red team notifies blue team that a round has *started* only if agreed:
  [yes / no — surprise rounds]
- Each round ends when: blue team detects + remediates + retests, OR
  either party calls it.

## 5. Data

All names, account numbers, and SSNs in this lab are synthetic and
have no connection to any real person or organization. No real NPI
is ever introduced into the environment.

## 6. Changes

This document is versioned. Either party can propose changes between
rounds; both must agree before the next round begins.

---
Signed (by committing this file):
Red team: [NToaster]  ·  Blue team: [handle]  ·  [11/2/2026]
