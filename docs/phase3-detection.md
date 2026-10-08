# Phase 3 - Détection (Blue Team)

## Objectif
Déployer un SIEM (Wazuh) et vérifier qu'il détecte une attaque réelle
lancée depuis le lab.

## Architecture
- VM dédiée Ubuntu Server 24.04 (192.168.188.5), 4 Go RAM, 2 CPU
- Wazuh 4.9.2 installé en all-in-one (manager + indexer + dashboard)
- Le manager surveille les logs de sa propre machine (auth.log, etc.)

## Scénario d'attaque
Depuis Kali (192.168.188.4), brute-force SSH contre le serveur Wazuh :
    hydra -l ismail -P rockyou.txt ssh://192.168.188.5 -t 1

## Détection obtenue
Dans Wazuh > Threat Hunting :
- 266 événements "Authentication failure" en quelques minutes
- Pic d'activité clairement visible sur la timeline
- Classification automatique MITRE ATT&CK : Brute Force (T1110),
  Password Guessing, SSH
![Détection du brute-force SSH dans Wazuh - MITRE ATT&CK](../screenshots/phase3-detection.png)

## Lecture
Le SIEM transforme des centaines de lignes de logs brutes en une alerte
lisible et catégorisée. C'est ce qui permet à un analyste de réagir vite.

## Remédiation (côté défense)
- Limiter les tentatives SSH (fail2ban, MaxAuthTries)
- Désactiver l'authentification par mot de passe (clés SSH uniquement)
- Changer le port SSH par défaut, restreindre par pare-feu
- Mettre en place des alertes temps réel sur seuil d'échecs