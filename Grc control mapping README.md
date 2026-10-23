# GRC Control Mapping - Home Lab to ISO 27001 & Cyber Essentials

A short governance, risk and compliance (GRC) exercise that maps the security
controls I built in my home labs to two recognised frameworks: the **UK Cyber
Essentials** five technical controls and **ISO/IEC 27001:2022 Annex A**. It also
includes a simple **risk register**. The goal is to show that hands-on security
work can be expressed in the language of risk, controls and audit - the way a
GRC or Information Security team documents it.

> **Why this exists:** controls are only half the job; being able to map them to a
> framework, record the evidence, and reason about residual risk is what turns
> technical work into assurance. This document does that for the labs in my
> [GitHub portfolio](https://github.com/aap-soc?tab=repositories).

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Scope

The "environment" assessed is my self-built lab:

|                Asset                    |            Description                      |
|-----------------------------------------|---------------------------------------------|
|    Microsoft 365 / Entra ID tenant      | Identity, admin accounts, access reviews    |
|            AWS account                  |        IAM, KMS, S3, CloudTrail             |
|          Windows 10 endpoint            | Monitored by Wazuh; Defender; host firewall |
|           Ubuntu servers                | Wazuh SIEM; ufw firewall; file permissions  |
|            Kali Linux                   |          Testing / scanning host            |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Part A - Cyber Essentials: the five controls

Cyber Essentials assesses five technical control areas. For each, below is what I
implemented in the lab and an honest status.

| # | Cyber Essentials control | Evidence in my labs | Status |
|---|---|---|---|
| 1 | **Firewalls** | Windows Defender Firewall inbound rules (block/allow ICMP, verified with ping); `ufw` on Ubuntu (allow 22, deny 80) proven with Nmap (open → filtered) | **Met** |
| 2 | **Secure configuration** | M365 tenant hardened with separate Global Admin / Security Admin accounts; AWS S3 public access blocked; root MFA enforced; default/unneeded access removed | **Met** |
| 3 | **User access control** | Entra ID quarterly access review with audit evidence; least privilege and separation of duties; AWS IAM users/groups; Linux least-privilege permissions (chmod 640) | **Met** |
| 4 | **Malware protection** | Microsoft Defender XDR / Defender antivirus on the Windows endpoint | **Met** |
| 5 | **Security update management (patching)** | Not yet formally documented | **Gap - next step** |

**Identified gap & treatment:** patch/update management is the one control I have
not evidenced. *Planned action:* document applying OS and application updates on
the Windows and Ubuntu VMs, confirm auto-update settings, and record it here.
(Identifying and tracking a gap is itself part of the GRC process.)

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Part B - ISO/IEC 27001:2022 Annex A mapping

Annex A groups controls into four themes: **Organizational (A.5)**,
**People (A.6)**, **Physical (A.7)** and **Technological (A.8)**. The table maps my
lab activities to the most relevant controls.

| Lab activity (evidence) | ISO 27001:2022 Annex A control |
|---|---|
| Entra ID access review, removing unneeded access with audit-log evidence | **A.5.18** Access rights · **A.5.15** Access control |
| Separate Global Admin / Security Admin accounts (least privilege) | **A.5.3** Segregation of duties · **A.8.2** Privileged access rights |
| AWS IAM users/groups, root MFA | **A.8.5** Secure authentication · **A.8.3** Information access restriction |
| S3 SSE-KMS encryption; customer-managed KMS key | **A.8.24** Use of cryptography |
| CloudTrail logging with log-file validation (tamper-evident) | **A.8.15** Logging · **A.8.16** Monitoring activities |
| Wazuh SIEM detecting failed logons (rule 60122); PowerShell log parsing | **A.8.15** Logging · **A.8.16** Monitoring activities |
| Host firewalls (Windows Firewall, ufw); network segmentation of VMs | **A.8.20** Networks security · **A.8.21** Security of network services |
| Nmap scanning / identifying the vsftpd 2.3.4 vulnerability | **A.8.8** Management of technical vulnerabilities |
| Microsoft Defender antivirus | **A.8.7** Protection against malware |
| Phishing investigation: scope → contain → assess → escalate → close | **A.5.24** Incident management planning · **A.5.25** Assessment of events · **A.5.26** Response to incidents |
| Documented case notes, troubleshooting logs, lessons learned | **A.5.28** Collection of evidence · **A.6.8** Information security event reporting |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Part C - Simple risk register

A lightweight register for the lab assets. Risk rating = Likelihood × Impact
(Low / Medium / High).

| Asset | Threat | Vulnerability | Likelihood | Impact | Risk | Control / treatment (Annex A) |
|-------|--------|---------------|------------|--------|------|-------------------------------|
| Email / users | Phishing | User susceptibility | High | Medium | **High** | Email auth checks (SPF/DKIM/DMARC), user awareness, incident playbook (A.5.24–26) |
| Windows endpoint | Brute-force logon | Weak lockout policy | Medium | Medium | **Medium** | Monitor failed logons (Wazuh rule 60122), enforce lockout + MFA (A.8.5, A.8.16) |
| AWS S3 (sensitive data) | Data exposure | Public bucket access | Low | High | **Medium** | Block public access, SSE-KMS encryption (A.8.3, A.8.24) |
| Cloud admin identities | Account takeover | Single over-privileged admin | Low | High | **Medium** | Separation of duties, least privilege, MFA, access reviews (A.5.3, A.8.2, A.5.18) |
| All hosts | Exploited known flaw | Missing patches | Medium | High | **High** | Patch/update management *(gap — see Part A)* (A.8.8) |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Key takeaways

- The same lab work maps cleanly onto **both** Cyber Essentials and ISO 27001 -
  frameworks describe *outcomes*, and good controls satisfy several at once.
- **Evidence matters:** every control above has an artefact behind it (a screenshot,
  a config, a case note) in the linked repos — which is what an auditor asks for.
- The honest **gap** (patch management) and the two **High** residual risks
  (phishing, patching) are the items a real risk register would prioritise next.

*Frameworks referenced: ISO/IEC 27001:2022 Annex A; NCSC / IASME Cyber Essentials.*
