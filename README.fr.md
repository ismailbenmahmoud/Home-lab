# Home Lab - Cybersécurité offensive & défensive

[English](./README.md) · **Français**

Un lab de sécurité monté de zéro pour pratiquer l'attaque, la défense et la
détection dans un environnement isolé. Projet solo, documenté phase par phase.

> **Mention légale** : toutes les attaques sont menées exclusivement contre
> mes propres machines, dans un réseau isolé sans accès Internet. Aucune action
> n'est jamais dirigée vers des systèmes tiers.

---

## Objectif

Couvrir toute la chaîne d'une opération de sécurité :
**reconnaissance → exploitation → post-exploitation → détection**,
en jouant à la fois l'attaquant (Red Team) et le défenseur (Blue Team).

---

##  Architecture

```
             Réseau isolé — 192.168.188.0/24 (Host-Only, sans Internet)

   ┌─────────────────┐       attaques       ┌──────────────────────┐
   │   KALI LINUX    │ ────────────────────▶│   METASPLOITABLE 2   │
   │  (attaquant)    │                       │   (cible vulnérable) │
   │  192.168.188.4  │                       │   192.168.188.3      │
   └────────┬────────┘                       └──────────────────────┘
            │
            │ logs d'activité
            ▼
   ┌─────────────────────────────┐
   │     WAZUH SIEM (Ubuntu)     │  ◀── collecte, détecte et classe
   │   192.168.188.5             │      les attaques (MITRE ATT&CK)
   └─────────────────────────────┘
```

---

## Phases du projet

| Phase | Objectif | Outils | Résultat |
|-------|----------|--------|----------|
| 1 | Mise en place du lab isolé | VirtualBox | 2 VM isolées, connectivité vérifiée |
| 2 | Attaque (Red Team) | nmap, Metasploit, John the Ripper | Accès root + cassage de mots de passe |
| 3 | Détection (Blue Team) | Wazuh (SIEM) | Attaque détectée + classée MITRE ATT&CK |
| 4 | Honeypot | Cowrie, VPS (Hetzner) | 20 038 attaques réelles capturées & analysées en 3,5 jours |

Chaque phase est détaillée dans [`/docs`](./docs).

---

## Points techniques forts

- **Exploitation** : backdoor vsftpd 2.3.4 (CVE-2011-2523) → shell root
- **Post-exploitation** : extraction de `/etc/shadow` et cassage (rockyou + règles)
- **Détection** : Wazuh SIEM détectant un brute-force SSH en temps réel,
  avec classification automatique via le framework MITRE ATT&CK
- **Threat intelligence** : un honeypot Cowrie exposé sur Internet a capturé
  20 038 attaques venant de 573 IP uniques en 3,5 jours ; identification d'un
  botnet coordonné hébergé sur une infrastructure complaisante (AS198364) et
  de la technique de persistance par clé SSH (MITRE T1098.004)
- **Esprit défensif** : chaque faille exploitée est accompagnée de sa
  remédiation dans la documentation

---

## Compétences démontrées

Virtualisation · Réseau & segmentation · Test d'intrusion ·
Linux · Analyse de logs / SIEM · MITRE ATT&CK · Threat intelligence ·
Rédaction technique

---

## Structure du dépôt
Home-lab/
├── docs/ # Documentation détaillée par phase
├── configs/ # Fichiers de configuration
└── screenshots/ # Captures (scans, exploitation, alertes)

---

## Auteur

**Ismail Ben Mahmoud** - Étudiant ingénieur, majeure cybersécurité (ECE Paris)