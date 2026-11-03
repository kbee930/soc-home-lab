# soc-home-lab
Red vs. blue team home lab (AD, Splunk, Sysmon) with incident writeups mapped to NIST CSF and MITRE ATT&amp;CK.

SOC Home Lab

Self-built Active Directory lab for practicing SOC investigation, incident response, and detection engineering, documented round by round.

----Remember---- 

This is for FUN, applying skills, improving, and learning new skills as we go.

The Story

This lab simulates Shieldpoint Recovery, a fictional small debt collection agency that handles nonpublic personal information (NPI) but has no dedicated security staff. I'm the IT employee informally responsible for security, learning to detect, investigate, and respond to incidents as they happen, building the program from scratch.

A friend plays red team, attacking the environment without warning. I play blue team: investigating what happened, documenting findings, remediating the root cause, and retesting to confirm the fix holds. Each round gets harder,the environment changes and gets more secure.

Shieldpoint Recovery is a fictional company for training purposes, with no connection to any real organization. All data used in this lab (names, account numbers, SSNs) is synthetic.

Why I Built This

I work in enterprise alert triage, but I wanted deeper hands-on experience with:

	•	Writing my own SIEM searches and detections (not just consuming existing ones)
	•	Host-level investigation using Sysmon and Windows Event Logs
	•	Owning the full incident lifecycle: detect, investigate, contain, remediate, retest
	•	Applying NIST CSF and MITRE ATT&CK in a hands-on, repeatable way

This repo documents that process. 

Environment

Component	Role

Windows Server 2022	Domain Controller (shieldpoint.local)

Kali Linux	Red team attack box

Splunk Enterprise SIEM, log collection and search

Sysmon Endpoint telemetry on the DC

All systems run in VirtualBox on an isolated internal network. Red team connects remotely over a VPN, limited to the attacker VM only.

Frameworks

	•	NIST SP 800-61 – incident response process
	•	NIST CSF 2.0 – organizing findings and remediation
	•	MITRE ATT&CK – naming attacker techniques
	•	CIS Controls / Microsoft Security Baseline – hardening targets


Each Incident will have a writeup from both sides, that lives in /rounds/round-XX/, including the incident report, timeline, evidence, remediation, and retest results.

Repo Structure

/rounds/round-01/     incident report, timeline, remediation, retest

/ir-plan/             incident response plan (versioned as it matures)

/detections/          Splunk searches and Sysmon rules I've written

/hardening/           before/after configs, CIS-mapped checklists

/diagrams/             network and attack flow diagrams

About Me

Cybersecurity analyst with experience in enterprise alert triage, phishing/malware investigation, and SOC operations. 
