# Home Lab - Cybersécurité offensive & défensive

[English](./README.md) · **Français**

Laboratoire de sécurité monté de zéro pour pratiquer l'attaque, la défense
et la détection dans un environnement isolé. Projet mené en autonomie,
documenté phase par phase.

> **Cadre légal** : toutes les attaques sont menées exclusivement contre
> mes propres machines, dans un réseau isolé et coupé d'Internet. Aucune
> action n'est dirigée contre un système tiers.

---

## Objectif

Couvrir la chaîne complète d'une opération de sécurité :
**reconnaissance → exploitation → post-exploitation → détection**, en
jouant successivement l'attaquant (Red Team) et le défenseur (Blue Team).

---

## Architecture

```
                 Réseau isolé — 192.168.188.0/24 (Host-Only, sans Internet)

   ┌─────────────────┐       attaques        ┌──────────────────────┐
   │   KALI LINUX    │ ────────────────────▶ │   METASPLOITABLE 2   │
   │  (attaquant)    │                       │   (cible vulnérable) │
   │  192.168.188.4  │                       │   192.168.188.3      │
   └────────┬────────┘                       └──────────────────────┘
            │
            │ journaux d'activité
            ▼
   ┌─────────────────────────────┐
   │     WAZUH SIEM (Ubuntu)     │  ◀── collecte, détecte et classe
   │   192.168.188.5             │      les attaques (MITRE ATT&CK)
   └─────────────────────────────┘
```


---

## Phases du projet

| Phase | Thème | Outils | Résultat |
|-------|-------|--------|----------|
| 1 | Mise en place du lab isolé | VirtualBox | 2 VM isolées, communication validée |
| 2 | Attaque (Red Team) | nmap, Metasploit, John the Ripper | Accès root + crack de mots de passe |
| 3 | Détection (Blue Team) | Wazuh (SIEM) | Attaque détectée + classée MITRE ATT&CK |
| 4 | Honeypot *(à venir)* | Cowrie, VPS | Analyse d'attaques réelles d'Internet |

Chaque phase est détaillée dans [`/docs`](./docs).

---

## Points forts techniques

- **Exploitation** : backdoor vsftpd 2.3.4 (CVE-2011-2523) → shell root
- **Post-exploitation** : extraction et crack du `/etc/shadow` (rockyou + règles)
- **Détection** : SIEM Wazuh détectant un brute-force SSH en temps réel,
  avec classification automatique selon le framework MITRE ATT&CK
- **Démarche défensive** : chaque faille exploitée est accompagnée de sa
  remédiation dans la documentation

---

## Compétences mises en œuvre

Virtualisation · Réseau & segmentation · Pentest · Linux ·
Analyse de logs / SIEM · MITRE ATT&CK · Documentation technique

---

## Structure du dépôt

Home-lab/
├── docs/ # Documentation détaillée par phase
├── configs/ # Fichiers de configuration
└── screenshots/ # Captures (scans, exploitations, alertes)

---

## Auteur

**Ismail Ben Mahmoud** - Étudiant ingénieur, majeure Cybersécurité (ECE Paris)
