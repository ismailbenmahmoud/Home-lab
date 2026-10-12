# Home Lab - Offensive & Defensive Cybersecurity

**English** · [Français](./README.fr.md)

A security lab built from scratch to practice attack, defense and detection
in an isolated environment. Solo project, documented phase by phase.

> **Legal notice**: all attacks are performed exclusively against my own
> machines, inside an isolated network with no Internet access. No action is
> ever directed at third-party systems.

---

## Goal

Cover the full chain of a security operation:
**reconnaissance → exploitation → post-exploitation → detection**,
playing both the attacker (Red Team) and the defender (Blue Team).

---

##  Architecture

```
             Isolated network — 192.168.188.0/24 (Host-Only, no Internet)

   ┌─────────────────┐       attacks        ┌──────────────────────┐
   │   KALI LINUX    │ ────────────────────▶│   METASPLOITABLE 2   │
   │  (attacker)     │                       │  (vulnerable target) │
   │  192.168.188.4  │                       │   192.168.188.3      │
   └────────┬────────┘                       └──────────────────────┘
            │
            │ activity logs
            ▼
   ┌─────────────────────────────┐
   │     WAZUH SIEM (Ubuntu)     │  ◀── collects, detects and classifies
   │   192.168.188.5             │      attacks (MITRE ATT&CK)
   └─────────────────────────────┘
```

---

## Project phases

| Phase | Focus | Tools | Outcome |
|-------|-------|-------|---------|
| 1 | Isolated lab setup | VirtualBox | 2 isolated VMs, connectivity verified |
| 2 | Attack (Red Team) | nmap, Metasploit, John the Ripper | Root access + password cracking |
| 3 | Detection (Blue Team) | Wazuh (SIEM) | Attack detected + mapped to MITRE ATT&CK |
| 4 | Honeypot | Cowrie, VPS (Hetzner) | 20,038 real attacks captured & analyzed in 3.5 days |

Each phase is detailed in [`/docs`](./docs).

---

## Technical highlights

- **Exploitation**: vsftpd 2.3.4 backdoor (CVE-2011-2523) → root shell
- **Post-exploitation**: `/etc/shadow` extraction and cracking (rockyou + rules)
- **Detection**: Wazuh SIEM detecting an SSH brute-force in real time,
  with automatic classification using the MITRE ATT&CK framework
- **Threat intelligence**: a Cowrie honeypot exposed on the Internet captured
  20,038 attacks from 573 unique IPs in 3.5 days; identified a coordinated
  botnet hosted on bulletproof infrastructure (AS198364) and the SSH-key
  persistence technique (MITRE T1098.004)
- **Defensive mindset**: every exploited flaw comes with its remediation
  in the documentation

---

## Skills demonstrated

Virtualization · Networking & segmentation · Penetration testing ·
Linux · Log analysis / SIEM · MITRE ATT&CK · Threat intelligence ·
Technical writing

---

## Repository structure
Home-lab/
├── docs/ # Detailed documentation per phase
├── configs/ # Configuration files
└── screenshots/ # Captures (scans, exploitation, alerts)

---

## Author

**Ismail Ben Mahmoud** - Engineering student, Cybersecurity major (ECE Paris)