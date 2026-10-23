# Cyber Essentials — The Five Controls (Reference)

A quick reference to the five Cyber Essentials technical controls, how each one
works, and where I have applied it in my labs. It pairs with the Cyber Essentials
self-assessment in **Part A** of the [GRC Control Mapping](./README.md).

**Memory hook:** *“Firewalls Stop Unwanted Access & Malware”* → **F**irewalls ·
**S**ecure configuration · **U**pdates · **A**ccess control · **M**alware protection.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## 1. Firewalls
A filter between your devices and the internet that checks data both ways and
allows or blocks it by rules.

- **Two kinds:** a *boundary firewall* (in your router or a hardware box - protects
  the whole network) and a *software/personal firewall* (on each device - built
  into Windows/macOS).
- Must be **configured correctly** — useless if left on defaults or switched off.
- Software firewalls also stop malware spreading device-to-device.

*My evidence:* configured Windows Defender Firewall and `ufw` rules, proven with
ping and Nmap (open → filtered).

## 2. Secure Configuration
Devices and cloud services aren't secure out of the box — you harden them.

- **Remove defaults:** change default passwords, delete unnecessary accounts,
  uninstall unused software.
- **Lock every device** with a 6+ character PIN/password or biometric.
- **Disable autorun/autoplay** so USBs/DVDs can't auto-install software.
- **Close unnecessary open ports;** understand and document what is exposed.

*My evidence:* separate admin accounts, S3 public access blocked, default/unneeded
access removed in M365 and AWS labs.

## 3. Security Update Management (Patching)
Keep all software patched so criminals can't exploit known holes.

- **Key rule: install critical/high-risk updates within 14 days** of release
  (critical/high = **CVSS 7.0+** — see below).
- Turn on **automatic updates**; keep OS, firmware, browsers, apps, antivirus and
  hypervisors current.
- **Remove unsupported / end-of-life software** (e.g. Windows 7) - no patches =
  not certifiable.
- Keep a **software asset inventory** so you know what you have and whether it's
  supported.

*My evidence:* identified as a gap in this GRC mapping - closing it by documenting
`apt upgrade` / Windows Update.

## 4. User Access Control
Give people only the access their role needs — no more. (The biggest control area.)

- **Unique accounts** for everyone, no shared accounts, so actions are traceable.
- **Separate admin and standard accounts** - never do email/browsing as admin; a
  standard account blocks most malware installs.
- **Delete unused and guest accounts.**
- **MFA - three factor types:** something you *know* (password), something you
  *have* (phone/token), something you *are* (biometric). Two of the same type isn't MFA.
- **Passwords:** 8+ characters *with* MFA, or 12+ *without* (or 8+ with a
  common-password deny list). Prefer three random words; don't force regular
  expiry or complexity.
- **Brute-force protection:** lock or throttle after no more than 10 failed attempts.
- **Stat to quote:** MFA makes an account **99.9% less likely** to be compromised (Microsoft).

*My evidence:* Entra ID access reviews, least privilege, separation of duties, MFA
on the root account.

## 5. Malware Protection
Anti-malware software that detects and stops malicious code.

- **Windows Defender meets Cyber Essentials** — no need to buy a product.
- Two detection methods: **signature** (matches known malware) and **heuristic**
  (spots suspicious behaviour in new malware).
- Set it to **scan files on access** and to **block known malicious websites**.
- **Mobile devices:** install apps only from official app stores / an approved list.
- Malware usually arrives via phishing, dodgy ads, untrusted downloads or USB sticks.

*My evidence:* Microsoft Defender on the endpoint; phishing investigation showing
how malware is delivered.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## What is CVSS?

**CVSS (Common Vulnerability Scoring System)** is the open industry standard for
rating how severe a software vulnerability is. Each vulnerability gets a score from
**0.0 to 10.0** based on how easy it is to exploit and how much damage it could
cause (current versions v3.1 and v4.0, maintained by FIRST.org). Higher = more urgent.

| CVSS score |           Severity                   |
|------------|--------------------------------------|
| 0.0        |             None                     |
| 0.1 – 3.9  |             Low                      |
| 4.0 – 6.9  |            Medium                    |
| 7.0 – 8.9  | **High**  ← patch within 14 days     |
| 9.0 – 10.0 | **Critical**  ← patch within 14 days |

**Why it matters for Cyber Essentials:** the 14-day patch rule applies to anything
scored **7.0 or above** (High or Critical). It's the same scoring used in Qualys
Vulnerability Management Foundations, alongside EPSS (probability of exploitation)
and CISA KEV (known exploited vulnerabilities) to prioritise what to fix first.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## One-line version

1. **Firewalls** - filter traffic in/out, configure them, close what's not needed.
2. **Secure config** - strip defaults, lock devices, close ports.
3. **Updates** — patch critical/high within 14 days, remove end-of-life software.
4. **Access control** - unique accounts, least privilege, separate admin, MFA, strong passwords.
5. **Malware protection** - anti-malware on, scan on access, app stores only for mobile.

*Source: NCSC / IASME Cyber Essentials — The Five Controls.*
